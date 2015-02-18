Swithmode expands down from the L-3 network packet handling to L-2 frame handling. In this mode, Ipop nodes function as a switch and make all the connected IPOP nodes and associated interface virtually be on  the same L2 link layer. 

This feature is not complete with the latest release yet. So you can either download from the below link or compile by your self with the latest "devel" branch. 

(for Ubuntu)
http://www.acis.ufl.edu/~xetron/ipop/ipop_ubuntu.tar.gz

(for CentOS 7)
http://www.acis.ufl.edu/~xetron/ipop/ipop_centos7.tar.gz



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
If the lxcbr0 address does not update, reboot your machine. 

After tincan connection established, you can ping from 10.0.3.5 to ping 10.0.3.7.  


