These instructions are for Ubuntu 12.04. 

If you use Android Virtual Device(AVD), your VM should have 1GB RAM or more.
You can create such a VM on
[FutureGrid](http://manual.futuregrid.org/openstackgrizzly.html).

If you use real Android devices, your devices must be rooted to run SocialVPN.
These instructions are tested for Android 3.0(Honeycomb) and 4.4.2(KitKat).

## Preparation

### Install necessary packages

1.  Update Debian/Ubuntu repo

    ```bash
    sudo apt-get update
    sudo apt-get install openjdk-7-jdk libc6:i386 libncurses5:i386 libstdc++6:i386
    ```

2.  Download and extract Android SDK

    ```bash
    wget http://dl.google.com/android/android-sdk_r22.0.1-linux.tgz
    tar xzvf android-sdk_r22.0.1-linux.tgz
    cd android-sdk-linux
    ```

3.  Download platform-tools and Android 4.1.2 qemu images (this takes a while)

    ```bash
    tools/android update sdk -u -t platform-tools,android-16,sysimg-16
    ```

### Download SocialVPN and dependencies

1.  Download SocialVPN and Python 2.7 for android

    ```bash
    wget -O ipop-14.07.0-android.tar.gz http://goo.gl/imA1Tm
    wget http://www.acis.ufl.edu/~ipop/files/python27.tgz
    tar xzvf python27.tgz; tar xzvf ipop-14.07.0-android.tar.gz
    ```

2.  Update the `config.json` file with proper credentials. For SocialVPN, you
    do not have to change the *ip4* address.


    ```bash
    cd ipop-14.07.0-arm_android
    cat config.json
    {
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com",
        "ip4": "172.31.0.100",
        "ip4_mask": 24,
        "tincan_logging": 0,
        "controller_logging": "DEBUG"
    }
    ```

## I. Running SocialVPN on Android Emulator
### Instantiate Android Virtual Device

1.  Define and create Android Virtual Device (AVD) (this also takes a while)

    ```bash
    tools/android create avd -n svpn-android-4.1 -t android-16 -c 1024M --abi armeabi-v7a
    ```

2.  Launch the newly created AVD

    ```bash
    tools/emulator64-arm -avd svpn-android-4.1 -no-window -no-audio -no-skin &> log.txt &
    ```

3.  Wait about one minute and test emulator is running with following command (a
    list of network devices along with ip addresses should appear)

    ```bash
    platform-tools/adb shell netcfg
    ```

### Prepare directories and SocialVPN files 
1.  Create directories for SocialVPN files. Android Emulator automatically gives you root access from adb shell.

    ```bash
    platform-tools/adb shell
    mkdir /data/ipop; chmod 777 /data/ipop
    exit
    platform-tools/adb shell mkdir data/ipop/python27
    ```

2.  Use `adb push` to copy downloaded files to AVD

    ```bash
    platform-tools/adb push ipop-14.07.0-android /data/ipop
    platform-tools/adb push python27 /data/ipop/python27
    ```

### Run SocialVPN

1.  Access the AVD shell and go to SocialVPN directory

    ```bash
    platform-tools/adb shell
    cd /data/ipop
    ```

2.  Launch SocialVPN

    ```bash
    chmod 755 ipop-tincan
    ./ipop-tincan 1> out.log 2> err.log &
    ```

3.  Log into XMPP (Google Chat or Jabber.org) using credentials

    ```bash
    sh start_controller.sh svpn_controller.py -c config.json &> log.txt &
    ```

4.  Check the network devices and ip address for your device

    ```bash
    netcfg
    ```

   By default, addresses are assigned dynamically on a round-robin fashion. Alternatively, you can assign addresses for your peers yourself through an additional configuration file. Please refer to our [[FAQs|FAQs]] for details.

### Close SocialVPN

1.  Kill SocialVPN process and terminate the AVD

    >   It is important to kill svpn-jingle-android process in order to exit
    >   shell.

    ```bash
    jobs
    kill %1 %2 # or the appropriate job number
    exit
    platform-tools/adb shell emu kill
    ```

## II. Running SocialVPN on Android Device
### Prepare directories and SocialVPN files
1.  Create directories for SocialVPN files. Main difference with running SocialVPN on Android Emulator is that most commands in this instruction must run as root. You are able to get root access per a terminal by typing `su`.

  ```bash
    platform-tools/adb shell
    su
    mkdir /data/ipop
    cd /data/ipop
    mkdir data/ipop/python27
    ```

2.  Create directories for copying downloaded files. First difference is that every file copy into the device must be through `/sdcard`

 ```bash

    cd ../../sdcard
    mkdir ipop
    mkdir python
    ```

3.  Use `adb push` to copy downloaded files to the device. First, copy SocialVPN files to `sdcard/ipop` and `sdcard/python27`.

    ```bash
    platform-tools/adb push ipop-14.07.0-android /sdcard/ipop
    platform-tools/adb push python27 /sdcard/python27
    ```

4. Change the binary file's mode as executable

    ```bash
    su
    cd sdcard
    chmod 755 ipop/ipop-tincan
    chmod 755 python27/files/python/bin/python
    ```

5. Move downloaded files from `sdcard` to `data`

    ```bash
    cp -R python27 /data/ipop/python27
    cp -R ipop /data/ipop
    ```

### Run SocialVPN
   
   Running SocialVPN on Android devices is same as the case of Android Virtual Device.

1.  Launch SocialVPN

    ```bash
    cd /data/ipop
    ./ipop-tincan 1> out.log 2> err.log &
    ```

2.  Log into XMPP (Google Chat or Jabber.org) using credentials

    ```bash
    sh start_controller.sh svpn_controller.py -c config.json &> log.txt &
    ```

3.  Check the network devices and ip address for your device

    ```bash
    netcfg
    ```
**Run SocialVPN on another machine using same credentials and they will connect
with each other.**

### Close SocialVPN

1.  Find SocialVPN process id and kill it

    ```bash
    ps | grep ipop-tincan
    kill [SocialVPN pid]
    ```
