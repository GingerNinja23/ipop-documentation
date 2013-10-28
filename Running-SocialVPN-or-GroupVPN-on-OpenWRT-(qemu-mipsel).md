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

2.  Launch the newly created AVD

    ```bash
    tools/emulator64-arm -avd svpn-android-4.1 -no-window -no-audio -no-skin \
                         &> log.txt &
    ```

3.  Wait about one minute and test emulator is running with following command (a
    list of network devices along with ip addresses should appear)

    ```bash
    platform-tools/adb shell netcfg
    ```

### Download and run Android SocialVPN

1.  Create directory for socialvpn files

    ```bash
    platform-tools/adb shell mkdir data/svpn
    platform-tools/adb shell mkdir data/svpn/python27
    ```

2.  Download socialvpn and Python 2.7 for android

    ```bash
    wget -O svpn-arm.tgz http://goo.gl/eBrvy1
    wget -O python27.tgz http://goo.gl/jjJxyd
    tar xzvf python27.tgz; tar xzvf svpn-arm.tgz
    ```

3.  Use `adb push` to copy downloaded files to AVD

    ```bash
    platform-tools/adb push svpn-arm /data/svpn
    platform-tools/adb push python27 /data/svpn/python27
    ```

4.  Access the AVD shell and go to svpn directory

    ```bash
    platform-tools/adb shell
    cd /data/svpn
    ```

5.  Launch socialvpn

    ```bash
    chmod 755 svpn-jingle-android
    ./svpn-jingle-android &> log.txt &
    ```

6.  Log into XMPP (Google Chat or Jabber.org) using credentials

    ```bash
    sh start_controller.sh username password xmpp-host
    ```

7.  Check the network devices and ip address for your device

    ```bash
    netcfg
    ```

8.  Kill socialvpn process and terminate the AVD

    >   It is important to kill svpn-jingle-android process in order to exit
    >   shell.

    ```bash
    jobs
    kill %1 # or the appropriate job number
    exit
    platform-tools/adb shell emu kill
    ```

Run socialvpn on another machine using same credentials and they will connect
with each other.
