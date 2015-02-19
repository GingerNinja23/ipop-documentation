In switchmode, IPOP supports handling of layer-2 (L2) Ethernet frames. In this mode, IPOP nodes function as a switch and make all the connected IPOP nodes and associated interface virtually be on the same L2 link layer. 

This feature is under development and not synced with the latest release yet. You may either download binaries from the links below, or compile by yourself from the latest "devel" branch. 

(for Ubuntu)
http://www.acis.ufl.edu/~xetron/ipop/ipop_ubuntu.tar.gz

(for CentOS 7)
http://www.acis.ufl.edu/~xetron/ipop/ipop_centos7.tar.gz



Note that in switchmode, the assignment of both IP and MAC addresses to virtual network interfaces needs to be done carefully to avoid conflicts. 

### Testing

The configuration of IPOP in switchmode is to a large extent the same - the main difference is that IPOP's tap device should be bound to a bridge on the L2 segments that you plan to inter-connect.

To test IPOP with switchmode test, we recommend following the example below using LXC containers with ubuntu 14.04 - this is the easiest way to run containers in VM. But, since switchmode works with Linux bridge, it works with any other cloud solutions such as openstack, ec2. 

First, bring up a Ubuntu VM and install LXC in it:

```
sudo apt-get update
sudo apt-get install lxc
```

If this command:
```
sudo lxc-ls --fancy
```
emits an error, update packages with:

```
sudo apt-get upgrade
```
Then, create an LXC instance and clone it:
```
sudo lxc-create -t ubuntu -n c0
sudo lxc-clone c0 c1
sudo lxc-clone c0 c2
...
```

The following diagram illustrates an example where you'd repeat this process in two VMs: 

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


