
This file explains the main components of controller design.

```python
class UdpServer(object):
    def __init__(self, user, password, host, ip4):

        # this dict stores the local user state
        self.state = {}

        # this dict store the state for each peer
        self.peers = {}

        # this set keeps track of unique
        self.peerlist = set()

        # this creates the UDP socket for communication with tincan
        if socket.has_ipv6:
            self.sock = socket.socket(socket.AF_INET6, socket.SOCK_DGRAM)
        else:
            self.sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        self.sock.bind(("", 0))

        # creates a random 20-byte uid and translates it to a hex string
        uid = binascii.b2a_hex(os.urandom(CONFIG["uid_size"]/2))

        # enables logging
        do_set_logging(self.sock)

        # sets the callback in tincan in order to receive event notifications
        do_set_cb_endpoint(self.sock, self.sock.getsockname())

        # configures the ipop interface and sets uid for XMPP network
        do_set_local_ip(self.sock, uid, ip4, gen_ip6(uid))

        # connects to XMPP service
        do_register_service(self.sock, user, password, host)

        # requests tincan to get state
        do_get_state(self.sock)

    def create_connection(self, uid, data, overlay_id, sec, cas, ip4):
        # keeps track of unique peers
        self.peerlist.add(uid)

        # creates a new connection to a peer
        do_create_link(self.sock, uid, data, overlay_id, sec, cas)

        # assigns an ip address to a remote peer
        do_set_remote_ip(self.sock, uid, ip4, gen_ip6(uid))

    def trim_connections(self):
        # this function is called about every 30 seconds and deletes
        # offline connections, it is important to delete old connections
        # because tincan will not allow reconnections if old connections
        # are still around
        for k, v in self.peers.iteritems():
            if "fpr" in v and v["status"] == "offline":
                if v["last_time"] > CONFIG["wait_time"] * 2:
                    do_trim_link(self.sock, k)

    def serve(self):
        # waits for incoming connections
        socks = select.select([self.sock], [], [], CONFIG["wait_time"])
        for sock in socks[0]:
            # receive packet from tincan
            data, addr = sock.recvfrom(CONFIG["buf_size"])
            if data[0] == '{':
                # transforms input to python objects
                msg = json.loads(data)
                logging.debug("recv %s %s" % (addr, data))

                # get message type from object
                msg_type = msg.get("type", None)

                # this is the local state object, so we save it
                if msg_type == "local_state": self.state = msg

                # this is a peer state object, so we save it too
                elif msg_type == "peer_state": self.peers[msg["uid"]] = msg

                # we ignore connection status notification for now
                elif msg_type == "con_stat": pass

                # we create a connection if we see these types of messages
                elif msg_type == "con_req" or msg_type == "con_resp":
                    fpr_len = len(self.state["_fpr"])
                    fpr = msg["data"][:fpr_len]
                    cas = msg["data"][fpr_len + 1:]

                    # gets an IP address for the new connection
                    ip4 = gen_ip4(msg["uid"], self.peerlist, self.state["_ip4"])

                    # create a connection from the con_req/con_resp
                    self.create_connection(msg["uid"], fpr, 1, CONFIG["sec"],
                                           cas, ip4)
```
