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

## Configuring OpenWRT for SocialVPN

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

## Download and configure SocialVPN

1.  Download socialvpn/groupvpn and extract for OpenWRT

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-openwrt-malta_14.01.pre1.tgz
    tar xvzf ipop-openwrt-malta_14.01.pre1.tgz
    cd ipop-openwrt-malta_14.01.pre1.tgz
    ```

2.  Update config file with appropriate credentials

    ```bash
    {
        "ip4": "172.31.0.100",
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com"
    }
    ```

## Running SocialVPN

1.  Launch ipop-tincan

    ```bash
    ./ipop-tincan 1> out.log 2> err.log &
    ```

2.  Start the appropriate controller

    a.   For SocialVPN

    ```bash
    ./svpn_controller.py -c config.json
    ```

    a.   For GroupVPN

    ```bash
    ./gvpn_controller.py -c config.json
    ```

    [[controller.png]]

3.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

    [[ifconfig.png]]

## Closing SocialVPN

1.  Kill socialvpn or groupvpn

    ```bash
    ps
    kill <process-id-ipop-tincan>
    kill <process-id-svpn_controller.py>
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**
