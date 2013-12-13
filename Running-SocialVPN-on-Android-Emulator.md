These instructions are for Ubuntu 12.04. Your VM should have 1GB RAM or more.

You can create such a VM on
[FutureGrid](http://manual.futuregrid.org/openstackgrizzly.html).

## Install necessary packages

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

## Instantiate Android Virtual Device

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

## Download SocialVPN and dependencies

1.  Create directory for socialvpn files

    ```bash
    platform-tools/adb shell mkdir data/ipop
    platform-tools/adb shell mkdir data/ipop/python27
    ```

2.  Download socialvpn and Python 2.7 for android

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-android_14.01.pre2.tgz
    wget http://www.acis.ufl.edu/~ptony82/ipop/python27.tgz
    wget http://github.com/ipop-project/ipop-scripts/raw/master/start_controller.sh
    tar xzvf python27.tgz; tar xzvf ipop-android_14.01.pre2.tgz
    mv start_controller.sh ipop-android_14.01.pre2
    ```

3.  Update the `config.json` file with proper credentials. For SocialVPN, you
    do not have to change the *ip4* address.


    ```bash
    cd ipop-android_14.01.pre2
    cat config.json
    {
        "ip4": "172.31.0.100",
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com"
    }
    ```

3.  Use `adb push` to copy downloaded files to AVD

    ```bash
    platform-tools/adb push ipop-android_14.01.pre2 /data/ipop
    platform-tools/adb push python27 /data/ipop/python27
    ```

## Running SocialVPN in Android Emulator

1.  Access the AVD shell and go to svpn directory

    ```bash
    platform-tools/adb shell
    cd /data/ipop
    ```

2.  Launch socialvpn

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

## Closing SocialVPN

1.  Kill socialvpn process and terminate the AVD

    >   It is important to kill svpn-jingle-android process in order to exit
    >   shell.

    ```bash
    jobs
    kill %1 %2 # or the appropriate job number
    exit
    platform-tools/adb shell emu kill
    ```

**Run socialvpn on another machine using same credentials and they will connect
with each other.**
