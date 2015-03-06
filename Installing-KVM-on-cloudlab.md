This documents shows steps to installing KVM on cloudlab.

1. Create baremetal instance on cloudlab. 
2. SSH to the instance.
3. Install required packages
    sudo apt-get update; sudo apt-get install qemu-kvm libvirt-bin
4. After installation you should see this 
```
$ sudo virsh list
 Id    Name                           State
----------------------------------------------------

$ sudo brctl show
bridge name	bridge id		STP enabled	interfaces
virbr0		8000.000000000000	yes		
$
```
5. Thanksfully, somehow it sets up all the NAT configuration for guests instance. You can see the masquarading on postrouting. So, you can simply attach your guest interface to bridge virbr0. 
```
$ sudo iptables -t nat --list
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination         

Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         

Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination         
RETURN     all  --  192.168.122.0/24     base-address.mcast.net/24 
RETURN     all  --  192.168.122.0/24     255.255.255.255     
MASQUERADE  tcp  --  192.168.122.0/24    !192.168.122.0/24     masq ports: 1024-65535
MASQUERADE  udp  --  192.168.122.0/24    !192.168.122.0/24     masq ports: 1024-65535
MASQUERADE  all  --  192.168.122.0/24    !192.168.122.0/24    
$ 
```
6. Now create disk volume for instance. 
```
$ qemu-img create -f raw kvm.img 5G
```
7. Place ubuntu server ISO files. You can use **scp** command. 

8. Create instance with below config file. 





Referenced from this sites
[1] http://xmodulo.com/use-kvm-command-line-debian-ubuntu.html
