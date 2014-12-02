Ipop-tincan supports so called switch-mode with linux bridge. In this mode, ipop captures arp_request or arp_reply message in the bridge network and broadcast to all ipop peers. As a result, all the ipop peers and all virtual interfaces that are attached to linux bridge with ipop is in the same l2 layer. It is users caveat assigning IP address not to collide. 



This feature is not yet in main distribution. You can download it below for the test.

[ipop-tincan](http://www.acis.ufl.edu/~xetron/ipop-project/ipop-tincan)

[ipoplib.py](https://raw.githubusercontent.com/ipop-project/controllers/devel/src/ipoplib.py)

[gvpn_controller.py](https://raw.githubusercontent.com/ipop-project/controllers/devel/src/gvpn_controller.py)


Make it excutable
```
chmod +x ipop-tincan gvpn_controller.py
```

Install LXC in user VM

```
sudo apt-get update
sudo apt-get install lxc
```

If 
```
sudo lxc-ls --fancy
```
emits error then run

```
sudo apt-get upgrade
```

We are using below pictures as an example. 

![](http://www.acis.ufl.edu/~xetron/ipop-project/switchmode.png)

Changing lxcbr ip address. Modifies field "LXC_ADDR" of below two files.

```
/etc/default/lxc-net
/etc/init/lxc-net.conf
```

Then restart lxc network

```
sudo service lxc-net restart
```

Add below fields at controller config file. 
```
   "switchmode": 1,
   "icc": true,
   "icc_port": 30000,
```
Run ipop-tincan and gvpn_controller. 
Then, attach ipop tap device to lxcbr0 at both ends. 

```
sudo brctl addif lxcbr0 ipop
```

After tincan connection established, you can ping from 10.0.3.5 to ping 10.0.3.7.  


Known issues and bugs
 - ICMP redirect message
 - duplicate packet upon ping to ipop

# Switch Mode under the Hood

IPOP switch mode capture ARP packet and sends RPC to replicate it on remote peer’s network. Below will give explanation how the switch mode works with an example scenario. In this scenario, LXC Instance with Veth0(10.0.3.3) try to send ping packet to LXC instance veth3(10.0.3.104). 

![](http://www.acis.ufl.edu/~xetron/ipop-project/switchmode_underthehood.png)


① Upon ping command, veth0 sends arp request messages to all interfaces on lxcbr0.

② Ipop captures arp request message and create json format arp_request RPC call to all of its peers  

③ ipop receives RPC call and create arp request message and broadcast on local lxcbr0 network. 

④ target(10.0.3.103) instance receives the arp request messages and reply with arp reply message

⑤ ipop receives ARP reply message and broadcast arp_reply RPC call to all of its peers. 

⑥ ipop0 receives arp_reply RPC call and update local arp_table then broadcast ARP reply message to local lxcbr0. 

Now, ARP is solicited. From now on, IP packets destined to 10.0.3.104 is forwarded through P2P connection. Since the controller keeps the table of the ARP mapping, upon the addition on ARP table, ARP request is solicited locally without having to broadcast arp_request RPC and receive arp_reply RPC from peers. So when certain time expires then the O/S ARP cache evicted the mapping and sends the ARP request message again, ARP is solicited only in a locally manner not broadcasting the ARP request messages to its peers. 