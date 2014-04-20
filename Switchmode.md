Ipop-tincan supports so called switch-mode with linux bridge. In this mode, ipop captures arp_request or arp_reply message in the bridge network and broadcast to all ipop peers. Conceptually, all the ipop peers and all virtual interfaces that are attached to linux bridge with ipop is in the same l2 layer. It is users caveat assigning IP address not to collide. 

In this example, we are using linux bridge

![](http://www.acis.ufl.edu/~xetron/ipop-project/switchmode.png)


  ipop    lxcbr0   i0   i1
   |
---|---------------------------  

It is users caveat to not to collide 


Controller config file 
```
   "switchmode": 1,
   "icc": true,
   "icc_port": 30000,
```


attach ipop tap device to lxcbr0. 

```
sudo brctl addif lxcbr0 ipop
```


Known issues and bugs
 - ICMP redirect message
 - duplicate packet upon ping to ipop