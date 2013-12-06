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
    opkg update; opkg install python librt libstdcpp kmod-tun kmod-ipv6 libpthread wget
    ```

## Download and configure GroupVPN

1.  Download groupvpn and extract for OpenWRT

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-openwrt-malta_14.01.pre1.tgz
    tar xvzf ipop-openwrt-malta_14.01.pre1.tgz
    cd ipop-openwrt-malta_14.01.pre1.tgz
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

    * `router_mode`: should be set to true
    * `router_ip`: should be the network that IPOP will handle
    * `router_ip4_mask`: IPv4 network mask
    * `router_ip6_mask`: IPv6 network mask
    * `subnet_mask`: network mask for the router

## Running GroupVPN

1.  Launch ipop-tincan

    ```bash
    ./ipop-tincan 1> out.log 2> err.log &
    ```

2.  Start the appropriate controller

    ```bash
    ./gvpn_controller.py -c config.json
    ```

    [[controller.png]]

3.  Check the network devices and ip address for your device

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
