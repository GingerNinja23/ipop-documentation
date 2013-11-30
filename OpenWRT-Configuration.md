In GroupVPN router mode, users can have all of the benefits of GroupVPN
without having to install GroupVPN on each individual device. They 
automatically get GroupVPN connectivity by connecting to an OpenWRT 
router. OpenWRT needs to be configured to enable this feature.

## Update OpenWRT network

First, you need to add the GroupVPN interface (ipop) to the network file,
this network is configured for 192.168.0.0/16 therefore any packet destined
for that subnet will be sent to ipop interface.

```bash
cat <<EOF >> /etc/config/network
config interface ipop
        option ifname   ipop
        option proto    static
        option ipaddr   192.168.0.0
        option netmask  255.255.0.0
EOF
```

## Update OpenWRT iptables

```bash
cat <<EOF >> /etc/config/firewall
config zone
        option name             ipop
        option network          'ipop'
        option input            ACCEPT
        option output           ACCEPT
        option forward          ACCEPT

config forwarding
        option src              ipop
        option dest             lan
EOF
```

## Running GroupVPN on your OpenWRT router
Follow the instructions on this [[page|Running SocialVPN or GroupVPN on OpenWRT Emulator]].
When running GroupVPN, make sure you set your IP address to the corresponding
subnet of your br-lan device, using the lowest address in the subnet. For 
example, if your br-lan device has the ip address 192.168.1.1 then you should
select 192.168.1.0 as your GroupVPN IP address.
