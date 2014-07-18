*  "stun": List up STUN servers to use. If omitted default value as below is used. 
["stun.l.google.com:19302", "stun1.l.google.com:19302",
             "stun2.l.google.com:19302", "stun3.l.google.com:19302",
             "stun4.l.google.com:19302"],
* "turn": [],  # Contains dicts with "server", "user", "pass" keys
* "ip4": "172.16.0.1",
*  "localhost": "127.0.0.1",
* "ip6_prefix": "fd50:0dbc:41f2:4a3c",
*  "localhost6": "::1",
*  "ip4_mask": 24,
*  "ip6_mask": 64,
*  "subnet_mask": 32,
* "svpn_port": 5800,
*  "contr_port": 5801,
*  "local_uid": "",
*  "uid_size": 40,
*  "sec": True,
*   "wait_time": 15,
*   "buf_size": 65507,
*   "router_mode": False,
*    "on-demand_connection" : False,
*    "on-demand_inactive_timeout" : 600,
*  "tincan_logging": 1,
*  "controller_logging" : "INFO",
*  "icc" : False, # Inter-Controller Connection
*   "icc_port" : 30000,
*   "switchmode" : 0,
*  "trim_enabled": False,
*   "multihop": False,
*  "multihop_cl": 100, #Multihop connection count limit
*   "multihop_ihc": 3, #Multihop initial hop count
*   "multihop_hl": 10, #Multihop maximum hop count limit
*   "multihop_tl": 1,  # Multihop time limit (second)
*   "multihop_sr": True # Multihop source route