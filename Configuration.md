In this page, we explain parameters in a configuration file in details. Default value is specified in ipoplib.py file. 

### Mandatory fields

These fields are critical so must be defined in config file. 

*     **ip4**: IP address of IPOP node
*     **xmpp_username**:  
*     **xmpp_host**: 
*     **xmpp_password**: 


### Optional fields

If these fields are omitted, default value at ipoplib.py file is used. 

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