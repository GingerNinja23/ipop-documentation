In SocialVPN mode, IPOP allows multihop routing. This allows any node can send IP packet to any other node even though there is no direct P2P connection. At this point, multihop only allows IPv6 packet, since only this addressing practically guarantees the uniqueness of address of every nodes. 

### 1. Multihop Configuration

To enable the multihop, you should specify "multihop" to be true in config file. 

```
    "multihop": True, # Default False
```

### 2. Auxiliary parameters
Below example lists varies of parameter of multihop mode with its default value. These parameters are configurable through config file. 

```
    "multihop_cl": 100, #Multihop connection count limit
    "multihop_ihc": 3, #Multihop initial hop count
    "multihop_hl": 10, #Multihop maximum hop count limit
    "multihop_tl": 1,  # Multihop time limit (second)
    "multihop_sr": True # Multihop source route
```

1. multihop_cl: It is the direct connection count limit per node. If the node happen reach this limit, next new direct connection use multihop path rather than creating direct p2p connection
2. multihop_ihc: This value is the initial hop count of lookup_request message. The first lookup_request message floods the IPOP overlay network from direct neighbors to given value of range hopping count. 
3. multihop_hl: Hop count multiples by 2 unless lookup_reply message received. Flood hopping count in lookup_request message is limited by this parameter. 
4. multihop_tl: After flood the network with lookup_request message, it either waits given time then flood the network with next hop count or concede flooding in case of reaching the multihop_hl.
5. multihop_sr: This is multihop source route mode. If set to false, routing information is distributed across the hopping nodes reduces the overhead of source route header. In source route mode, routing information is stored at the source providing more fault-tolerance against link failures. 

