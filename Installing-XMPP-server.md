IPOP allows you to connect to existing XMPP/STUN/TURN services. It is also possible to deploy your own private Online Social Network and NAT traversal services using the ejabberd open-source software and an open-source TURN service.

**The ejabberd software also runs a STUN server as part of its implementation. Therefore ejabberd serves as both XMPP server and STUN server.**

With this setup, you can create your own users and user relationships directly on the ejabberd server. This is especially useful when running GroupVPN - it allows you to define your own groups and endpoint IDs.

These instructions have only been tested on Ubuntu 12.04

## Install and configure ejabberd

1.  Install ejabberd on Ubuntu 12.04

    ```bash
    sudo apt-get update
    sudo apt-get install ejabberd
    ```

2.  Update configuration

    ```bash
    sudo vi /etc/ejabberd/ejabberd.cfg

    #update lines with ipopuser and ejabberd host
    %% Admin user
    {acl, admin, {user, "ipopuser", "ejabberd"}}.

    %% Hostname
    {hosts, ["localhost", "ejabberd"]}.
    ```

3.  Add the following to the ejabberd.cfg file to enable STUN
    ```bash
    sudo vi /etc/ejabberd/ejabberd.cfg

    {listen,
     [
      {5222, ejabberd_c2s, [
                            {access, c2s},
                            {shaper, c2s_shaper},
                            {max_stanza_size, 65536},
                            %%zlib,
                            starttls, {certfile, "/etc/ejabberd/ejabberd.pem"}
                           ]},

      %% this is the new line to enable stun
      {{3478, udp}, ejabberd_stun, []},
    ```

3.  (Optional) Create a new self-signed certificate. If you change the hostname from _ejabberd_
    to something else, you need to create a new self-signed certificate and set CN (common name) to
    the new hostname that you have chosen and place it under /ect/ejabberd

    ```bash
    openssl req -x509 -nodes -days 3650 -newkey rsa:1024 -keyout ejabberd.pem -out ejabberd.pem
    ```

4.  Restart ejabberd service

    ```bash
    sudo service ejabberd restart
    ```

5.  Create default user

    ```bash
    sudo ejabberdctl register ipopuser ejabberd password
    ```

## Configure SocialVPN/GroupVPN

1.  Update config.json file to point to new STUN
    server, we recommend that you use the public IP address (not DNS) of the 
    STUN server, especially if server is running on a public cloud (i.e. 
    Amazon EC2)

    ```bash
    {
        "ip4": "172.31.0.100",
        "xmpp_username": "ipopuser@ejabberd",
        "xmpp_password": "password",
        "xmpp_host": "public-ip-of-ejabberd-vm",
        "stun": ["public-ip-of-ejabberd-vm:3478"]
    }
    ```

## Additional ejabberd configurations

-   You can add more users by going to `http://ip-address-of-vm:5280/admin`
    (click on Virtual Hosts ==\> ejabberd ==\> Users)

    >   username = ipopuser@ejabberd  
    >   password = password

-   If you are running this on the cloud, make sure to open ports (port 3478 is necessary because ejabberd
    runs a STUN server as well)

    > TCP - 5222, 5269, 5280  
    > UDP - 3478

---

You can also [[setup a TURN server|Installing TURN Server]] in cases where your
network firewall does not allow direct connections.
