This documents shows steps to installing KVM on cloudlab.

* Create baremetal instance on cloudlab. 
* SSH to the instance.
* Install required packages
```
$ sudo apt-get update; sudo apt-get install qemu-kvm libvirt-bin
```

* After installation you should see this 
```
$ sudo virsh list
 Id    Name                           State
----------------------------------------------------

$ sudo brctl show
bridge name	bridge id		STP enabled	interfaces
virbr0		8000.000000000000	yes		
$
```
* Thanksfully, somehow it sets up all the NAT configuration for guests instance. You can see the masquarading on postrouting. So, you can simply attach your guest interface to bridge virbr0. 
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
* Now create disk volume for instance. 
```
$ qemu-img create -f raw ubuntu.img 5G
```
* Place ubuntu server ISO files. You can use **scp** command. 

* Create instance with below config file. 
Change UUID field if you want. Check with path of img file and source ISO file. Interface type fields specifies bridge attaching. 

```
<domain type='kvm'>
 ** <name>ubuntu</name>**
**  <uuid>f5b8c05b-9c7a-3211-49b9-2bd635f7e2ab</uuid>**
  <memory>1048576</memory>
  <currentMemory>1048576</currentMemory>
  <vcpu>1</vcpu>
  <os>
    <type>hvm</type>
    <boot dev='cdrom'/>
  </os>
  <features>
    <acpi/>
  </features>
  <clock offset='utc'/>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>
  <devices>
    <emulator>/usr/bin/kvm</emulator>
    <disk type="file" device="disk">
      <driver name="qemu" type="raw"/>
  **    <source file="/users/kyuho/ubuntu.img"/>**
      <target dev="vda" bus="virtio"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x04" function="0x0"/>
    </disk>
    <disk type="file" device="cdrom">
      <driver name="qemu" type="raw"/>
      **<source file="/users/kyuho/ubuntu-14.04.2-server-amd64.iso"/>**
      <target dev="hdc" bus="ide"/>
      <readonly/>
      <address type="drive" controller="0" bus="1" target="0" unit="0"/>
    </disk>
    <controller type="ide" index="0">
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x1"/>
    </controller>
    <input type='mouse' bus='ps2'/>
    <graphics type='vnc' port='-1' autoport="yes" listen='0.0.0.0'/>
    <console type='pty'>
      <target port='0'/>
    </console>
    <interface type='bridge'>
      **<mac address='52:54:00:d1:6d:b9'/>**
      <source bridge='virbr0'/>
    </interface>
  </devices>
</domain>

```

* now you see the instance is running.
```
$ sudo virsh list
 Id    Name                           State
----------------------------------------------------
 5     ubuntu                         running

$ 
```

* Lets start installing Ubuntu on it. You may see port 5900 is open. This is for VNC client access. 
```
$ sudo netstat -nap
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:16505           0.0.0.0:*               LISTEN      2135/pubsubd    
tcp        0      0 0.0.0.0:33413           0.0.0.0:*               LISTEN      1679/rpc.statd  
**tcp        0      0 0.0.0.0:5900            0.0.0.0:*               LISTEN      5259/qemu-system-x8**
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1672/rpcbind
```

* With your local computer, use any of VNC client to access KVM guest. I happen to use xvnc4viewer. It will ask "server:". Type in public IP address of baremetal you created. 
```
$ xvnc4viewer
```

* Complete installing ubuntu.


Referenced from this sites
[1] http://xmodulo.com/use-kvm-command-line-debian-ubuntu.html
