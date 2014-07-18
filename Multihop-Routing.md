In SocialVPN mode, IPOP allows multihop routing. This allows any node can send IP packet to any other node even though there is no direct P2P connection. At this point, multihop only allows IPv6 packet, since only this addressing practically guarantees the uniqueness of address of every nodes. 

1. Multihop Configuration

To enable the multihop, you should specify below configuration in config file. 

```
    "multihop": True, #
    "multihop_cl": 100, #Multihop connection count limit
    "multihop_ihc": 3, #Multihop initial hop count
    "multihop_hl": 10, #Multihop maximum hop count limit
    "multihop_tl": 1,  # Multihop time limit (second)
    "multihop_sr": True # Multihop source route
```
2.