In this page, we explain parameters in a configuration file in details. Default value is specified in ipoplib.py file. 


* "stun": List up STUN servers to use. If omitted google public STUN server is used.  
* "turn": List TURN servers with server transport address username and password as dictonary keys
* "ip4": IP address of IPOP node
* "ip6_prefix": Prefix of IPv6 address. 
* "ip4_mask": IPv4 netmask in integer(i.e., 24 for 255.255.255.0)
* "ip6_mask": IPv6 netmask in integer
* "subnet_mask": 
* "svpn_port": Port number of TinCan UDP local server
* "contr_port": Port number of controller UDP local server
* "local_uid": Local UID. If not specified, it is randomly generated. 
* "uid_size": UID length
* "sec": DTLS security 
* "wait_time": TinCan socket blocking period. Controller sends get_state message in this period.
* "buf_size": Controller socket buffer size. 
* "router_mode": TinCan controller router mode. 
* "on-demand_connection" : IPOP only creates TinCan connection when there is actual packet needs to be transfered. 
* "on-demand_inactive_timeout" : If the TinCan connection is inactive this given time, the connection is discarded. 
* "tincan_logging": Logging verbosity of TinCan executable. 0 for Error, 1 for Info, 2 for Debug
* "controller_logging" : "Logging verbosity of TinCan controller. ("ERROR", "INFO", "DEBUG" and "PKTDUMP")
* "icc" : It stands for Inter-Controller Connection. Controller open UDP socket server for the communication among IPOP controller. (True/False)
* "icc_port" : Inter-Controller Connection UDP socket port number (default 30,000)
* "switchmode" : This sets switchmode (1 for enable and 0 for disable)
* "trim_enabled": 
* "multihop": Multihop mode (True/False)
* "multihop_cl": Multihop connection count limit
* "multihop_ihc": Multihop initial hop count
* "multihop_hl": Multihop maximum hop count limit
* "multihop_tl": Multihop time limit (second)
* "multihop_sr": Multihop source route


*     **ip4**: 
*     **xmpp_username**:  
*     **xmpp_host**: 
*     **xmpp_password**: 
*     **router_mode**: Explanation. If it is ''True'', it means that ... 
*     **router_ip**: The network that IPOP will handle
*     **router_ip4_mask**: IPv4 network mask
*     **router_ip6_mask**: IPv6 network mask
*     **subnet_mask**: network mask for the router
*     **tincan_logging**: Explanation. If it is ''0'', ...
*     **controller_logging**: Explanation. If it is ''DEBUG'', ...
*     **sec**: Explanation. If it is ''True'', ...



[Example 1]
```bash
    {
        "xmpp_username": "ipopuser@gmail.com",
        "xmpp_password": "12345678",
        "xmpp_host": "talk.google.com",
        "ip4": "172.31.0.100",
        "ip4_mask": 24,
        "tincan_logging": 0,
        "controller_logging": "DEBUG"
    }
```


[Example 2]
```bash
    {
        "ip4": "192.168.1.0",
        "xmpp_username": "ipopuser@gmail.com",
        "xmpp_host": "talk.google.com",
        "xmpp_password": "password",
        "router_mode": true,
        "router_ip": "192.168.0.0",
        "router_ip4_mask": 16,
        "router_ip6_mask": 64,
        "subnet_mask": 24
    }
```