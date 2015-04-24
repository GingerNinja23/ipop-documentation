This documentation applies to generic Openwrt based routers, has been tested on AR71XX, X86 and CNS3XX based platforms. For a high level view of switch mode refer to the following link--  
[switch-mode](https://github.com/ipop-project/documentation/wiki/Switchmode-%28new%29)  

## Get dependency packages  
```bash  
 opkg update; opkg install python-mini librt libstdcpp kmod-tun kmod-ipv6 libpthread wget  
``` 
If you have enough storage, one way is to have extFS on uSD card or USB drive, i would suggest installing the full python package. Also some times a reboot is required to bring kernel modules online.  

## Getting things started  

See building code section on wiki page to build the code for your platform, and download the latest controllers, if you have not done it yet.  
Once you have the binary and controllers we can start with configuration,to begin with one has to ensure that the subnet in which router allocates addresses to the client and the IPOP subnet and address are in the same range. Also ensure that addresses allocated to clients do not conflict with others in the IPOP network.Below is a sample config file.  
```bash
root@OpenWrt:/etc/controllers/src# cat config.json 
{
    "xmpp_username": "XXXXXX@dukgo.com",
    "xmpp_password": "password",
    "xmpp_host": "dukgo.com",
    "ip4": "192.168.1.40",
    "tincan_logging": 2,
    "ip4_mask": 24,
    "controller_logging": "DEBUG",
    "switchmode":1
}
```  
  
 Start tin-can and controller.  
```bash
 ./ipop-tincan 1> out.log 2> err.log &
 ./gvpn_controller.py -c config.json &> log.txt &
```  
Now you should be able to see the below interface up--  
```bash
ipop      Link encap:Ethernet  HWaddr 0E:2A:47:2B:D6:B5  
          inet addr:192.168.1.40  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fd50:dbc:41f2:4a3c:7497:a943:9524:d6f0/64 Scope:Global
          inet6 addr: fe80::c2a:47ff:fe2b:d6b5/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1280  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:5 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:500 
          RX bytes:0 (0.0 B)  TX bytes:518 (518.0 B)
```  

 Attach it to the bridged interface on your router.  
```python
root@OpenWrt:/etc/controllers/src# brctl show
bridge name	bridge id		STP enabled	interfaces
br-lan		7fff.00d01283f0dd	no		eth0
							wlan0
root@OpenWrt:/etc/controllers/src# brctl addif br-lan ipop
root@OpenWrt:/etc/controllers/src# brctl show
bridge name	bridge id		STP enabled	interfaces
br-lan		7fff.00d01283f0dd	no		eth0
							wlan0
							ipop
root@OpenWrt:/etc/controllers/src# 

``` 
Now your connections should be up and running in a few seconds.