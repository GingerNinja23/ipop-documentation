Detailed description on config file options are listed below. Default value is specified in ipoplib.py file. 

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
* "wait_time": 
* "buf_size": 
* "router_mode": 
* "on-demand_connection" : 
* "on-demand_inactive_timeout" :
* "tincan_logging": 
* "controller_logging" : "INFO",
* "icc" : False, # Inter-Controller Connection
* "icc_port" : 30000,
* "switchmode" : 0,
* "trim_enabled": False,
* "multihop": False,
* "multihop_cl": 100, #Multihop connection count limit
* "multihop_ihc": 3, #Multihop initial hop count
* "multihop_hl": 10, #Multihop maximum hop count limit
* "multihop_tl": 1,  # Multihop time limit (second)
* "multihop_sr": True # Multihop source route