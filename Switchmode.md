Ipop-tincan supports so called switch-mode with linux bridge. In this mode, ipop captures arp_request or arp_reply message in the bridge network and broadcast to all ipop peers. As a result, all the ipop peers and all virtual interfaces that are attached to linux bridge with ipop is in the same l2 layer. It is users caveat assigning IP address not to collide. 

We are using below pictures as an example. 

![](http://www.acis.ufl.edu/~xetron/ipop-project/switchmode.png)

Controller config file
```
   "switchmode": 1,
   "icc": true,
   "icc_port": 30000,
```

Then, attach ipop tap device to lxcbr0 at both ends. 

```
sudo brctl addif lxcbr0 ipop
```

After tincan connection established, you can ping from 10.0.3.5 to ping 10.0.3.7.  


Known issues and bugs
 - ICMP redirect message
 - duplicate packet upon ping to ipop