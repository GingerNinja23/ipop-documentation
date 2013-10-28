This uses Qemu mipsel to run OpenWRT malta (which is designed specifically for that)

### Install Qemu and running OpenWRT on your system

1.  Update Debian/Ubuntu repo

    ```bash
    sudo apt-get update
    sudo apt-get install qemu-system qemu-user qemu-utils
    ```

2.  Download OpenWRT malta kernel image

    ```bash
    wget http://downloads.openwrt.org/attitude_adjustment/12.09-rc1/malta/generic/openwrt-malta-le-vmlinux.elf
    ```

3.  Run OpenWRT image on Qemu (make sure you use ```-nographic`` flag), press enter to activate console

    ```bash
    qemu-system-mipsel -kernel openwrt-malta-le-vmlinux.elf -m 256 -nographic
    ```

### Configuring OpenWRT for SocialVPN

1.  Run DHCP on bridge interface br-lan to get internet connectivity and set up DNS server

    ```bash
    udhcpc -i br-lan
    echo "nameserver 8.8.8.8" >> /etc/resolv.conf
    ```

2.  Update packages list and install dependencies

    ```bash
    sed -i 's/le/generic/g' /etc/opkg.conf 
    sed -i 's/\/overlay/\/tmp/g' /etc/opkg.conf
    opkg update; opkg install python librt libstdcpp kmod-tun ip6tables wget
    ```

### Download and run binaries

1.  Download socialvpn/groupvpn and extract for OpenWRT

    ```bash
    wget -O ipop-openwrt.tgz --no-check-certificate http://goo.gl/RM6yvy
    tar xvzf ipop-openwrt.tgz
    cd ipop-openwrt
    ```

2.  Launch groupvpn

    ```bash
    ./ipop-tincan 1> out.log 2> err.log &
    ```

3.  Log into XMPP (e.g. Google Chat, Jabber.org, or your own private XMPP server) using username/password credentials and configuring a virtual IP address

    a.   For SocialVPN

    ```bash
    python svpn_controller.py username password xmpp-host
    ```

    a.   For GroupVPN

    ```bash
    python gvpn_controller.py username password xmpp-host ip-address
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

5.  Kill socialvpn or groupvpn

    ```bash
    ps
    kill <process-id-ipop-tincan>
    kill <process-id-svpn_controller.py>
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**