These instructions are for Ubuntu 12.04 or Debian Wheezy

This uses Qemu mipsel to run OpenWRT malta (designed specifically for Qemu)

## Run OpenWRT on Qemu

1.  Install qemu and necessary utils

    ```bash
    sudo apt-get update
    sudo apt-get install qemu-system qemu-user qemu-utils
    ```

2.  Download OpenWRT malta kernel image

    ```bash
    wget http://downloads.openwrt.org/attitude_adjustment/12.09-rc1/malta/generic/openwrt-malta-le-vmlinux.elf
    ```

3.  Run OpenWRT image on Qemu (make sure you use ```-nographic`` flag)

    ```bash
    qemu-system-mipsel -kernel openwrt-malta-le-vmlinux.elf -m 256 -nographic
    ```

## Configuring OpenWRT for GroupVPN

1.  Run DHCP on bridge interface br-lan to get internet connectivity and set up DNS server

    ```bash
    udhcpc -i br-lan
    echo "nameserver 8.8.8.8" >> /etc/resolv.conf
    ```

2.  Update packages list and install dependencies

    ```bash
    sed -i 's/le/generic/g' /etc/opkg.conf 
    sed -i 's/\/overlay/\/tmp/g' /etc/opkg.conf
    opkg update; opkg install python-mini librt libstdcpp kmod-tun kmod-ipv6 libpthread wget
    ```

## Download and configure GroupVPN

1.  Download groupvpn and extract for OpenWRT

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-openwrt-malta_14.01.pre3.tgz
    tar xvzf ipop-openwrt-malta_14.01.pre3.tgz
    cd ipop-openwrt-malta_14.01.pre3
    ```

2.  Update `config.json`. It is important to include the following configurations
    in your `config.json` file, you have to enable router_mode, along with 
    `router_ip4_mask`, `router_ip6_mask`, `subnet_mask`, and `router_ip`. 
    It should looks as the following:

    ```
    {
        "ip4": "192.168.1.0",
        "xmpp_username": "username@gmail.com",
        "xmpp_host": "talk.google.com",
        "xmpp_password": "password",
        "router_mode": true,
        "router_ip": "192.168.0.0",
        "router_ip4_mask": 16,
        "router_ip6_mask": 64,
        "subnet_mask": 24
    }
    ```

    * `ip4`: should be set to the subnet of the local LAN, you can find that information under `ect/config/dhcp`
    * `router_mode`: should be set to true
    * `router_ip`: should be the network that IPOP will handle
    * `router_ip4_mask`: IPv4 network mask
    * `router_ip6_mask`: IPv6 network mask
    * `subnet_mask`: network mask for the router

3.  Configure your OpenWRT system by adding the GroupVPN interface (ipop) to the 
    network file, this network is configured for 192.168.0.0/16 therefore any
    packet destined for that subnet will be sent to ipop interface.

    ```bash
    cat <<EOF >> /etc/config/network
    config interface ipop
            option ifname   ipop
            option proto    static
            option ipaddr   192.168.0.0
            option netmask  255.255.0.0
    EOF
    ```

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

## Running GroupVPN

1.  Launch ipop-tincan

    ```bash
    ./ipop-tincan 1> out.log 2> err.log &
    ```

2.  Start the appropriate controller

    ```bash
    ./gvpn_controller.py -c config.json &> log.txt &
    ```

3.  Check on the current status of your network using netcat

    ```bash
    netcat -u 127.0.0.1 5800
    {"m":"get_state"}
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

    [[ifconfig.png]]

## Closing GroupVPN

1.  Kill groupvpn

    ```bash
    ps
    kill <process-id-of-ipop-tincan>
    kill <process-id-of-gvpn_controller.py>
    ```

**Run groupvpn on another machine using same credentials and they will connect
with each other.**

## Running GroupVPN in Watchdog mode

It is common practice to use a watchdog process to monitor and respawn
long running processes. We have designed a simple watchdog process that
spawns ipop-tincan and respawns it up to three times if necessary.

Our watchdog process automatically starts the ipop-tincan, so that you 
do not have to run it separately. (the path of the binary should be specified
in configuration file). If the ipop-tincan crashes or is not responding, 
the watchdog process terminates the ipop-tincan process and starts it as a 
new process.

```
sudo ./watchdog.py -c config.json
```

