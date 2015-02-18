Swithmode expands down from the L-3 network packet handling to L-2 frame handling. In this mode, Ipop nodes function as a switch and make all the connected IPOP nodes and associated interface virtually be on  the same L2 link layer. 

This feature is not complete with the latest release yet. So you can either download from the below link or compile by your self with the latest "devel" branch. 

(for Ubuntu)
http://www.acis.ufl.edu/~xetron/ipop/ipop_ubuntu.tar.gz

(for CentOS 7)



It is users caveat assigning IP address not to collide. 

For the switchmode test, we use LXC with ubuntu.
We recommend to use LXC and ubuntu, since it is the easiest way to run containers in VM. But, since switchmode works with Linux bridge, it works with any other cloud solutions such as openstack, ec2 and etc. 

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
Then create LXC instances and clone it. 
```
sudo lxc-create -t ubuntu -n c0
sudo lxc-clone c0 c1
sudo lxc-clone c0 c2
...
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
   "switchmode": 1
```
Run ipop-tincan and gvpn_controller. 
Then, attach ipop tap device to lxcbr0 at both ends. 

```
sudo brctl addif lxcbr0 ipop
```

If you need to change the IP address of lxcbr0, then modify below files and restart the network service
```
sudo vi /etc/default/lxc-net
sudo vi /etc/init/lxc-net.conf
sudo service lxc-net restart
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