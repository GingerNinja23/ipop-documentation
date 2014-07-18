In this page, we explain parameters in a configuration file in details. Default value is specified in ipoplib.py file. 

### Mandatory fields

These fields are critical so must be defined in config file. 

*     **ip4**: IP address of IPOP node
*     **xmpp_username**: XMPP credential user ID
*     **xmpp_host**: XMPP server domain or IP address
*     **xmpp_password**: XMPP credential password 


### Optional fields

If these fields are omitted, default value at ipoplib.py file is used. 

*     **stun**: List up STUN servers to use. If omitted google public STUN server is used.  
*     **turn**: List TURN servers with server transport address username and password as dictonary keys
*     **ip6_prefix**: Prefix of IPv6 address. 
*     **ip4_mask**: IPv4 netmask in integer(i.e., 24 for 255.255.255.0)
*     **ip6_mask**: IPv6 netmask in integer
*     **router_mode**: IPOP router mode. If it sets True, the IPOP runs in router mode. It only works with OpenWRT.
*     **router_ip**: The IP identifier of IPOP router itself. This field only valid in router_ip mode.  
*     **router_ip4_mask**: IPv4 network mask
*     **router_ip6_mask**: IPv6 network mask
*     **subnet_mask**: network mask for the router
*     **tincan_logging**: Logging verbosity of TinCan executable. 0 for Error, 1 for Info, 2 for Debug
*     **controller_logging**: Logging verbosity of TinCan controller. ("ERROR", "INFO", "DEBUG" and "PKTDUMP")
*     **sec**: If set True, DTLS security is enabled.
*     **svpn_port**: Port number of TinCan UDP local server
*     **contr_port**: Port number of controller UDP local server
*     **local_uid**: Local UID. If not specified, it is randomly generated. 
*     **uid_size**: UID length
*     **wait_time**: TinCan socket blocking period. Controller sends get_state message in this period.
*     **buf_size**: Controller socket buffer size.  
*     **on-demand_connection** : IPOP only creates TinCan connection when there is actual packet needs to be transfered. 
*     **on-demand_inactive_timeout** : If the TinCan connection is inactive this given time, the connection is discarded. 
*     **icc** : It stands for Inter-Controller Connection. Controller open UDP socket server for the communication among IPOP controller. (True/False)
*     **icc_port** : Inter-Controller Connection UDP socket port number (default 30,000)
*     **switchmode** : This sets switchmode (1 for enable and 0 for disable)
*     **trim_enabled**: 
*     **multihop**: Multihop mode (True/False)
*     **multihop_cl**: Multihop connection count limit
*     **multihop_ihc**: Multihop initial hop count
*     **multihop_hl**: Multihop maximum hop count limit
*     **multihop_tl**: Multihop time limit (second)
*     **multihop_sr**: Multihop source route



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
[Example 3] 
```bash
{
    "stun": ["stun.l.google.com:19302"],
    "turn": [
        {"server": "129.114.33.157:19302", "user": "svpnjingle", "pass": "1234567890"}
    ],
    "ip4": "172.16.3.10",
    "localhost": "127.0.0.1",
    "ip6_prefix": "fd50:0dbc:41f2:4a3c",
    "localhost6": "::1",
    "svpn_port": 5800,
    "uid_size": 40,
    "sec": true,
    "wait_time": 30,
    "buf_size": 65507,
    "xmpp_username": "0@ejabberd",
    "xmpp_password": "0",
    "xmpp_host": "129.114.33.157",
    "tincan_path": "../../trunk/out/Release/ipop-tincan",
    "on-demand_connection": false,
    "on-demand_inactive_timeout": 100,
    "tincan_logging": 2,
    "switchmode": 1,
    "controller_logging": "PKTDUMP",
    "icc": true,
    "icc_port": 30000,
    "multihop": false,
    "network_ignore_list": ["virbr0", "wlan0"]
}

```