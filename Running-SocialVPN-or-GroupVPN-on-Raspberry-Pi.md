These instructions are for Raspbian (Raspberry Pi) compiled with vfp and hard
floating point.

## Download and configure SocialVPN

1.  Download groupvpn/socialvpn and extract for Raspberry Pi

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-rpi_14.01.pre1.tgz
    tar xvzf ipop-rpi_14.01.pre1.tgz
    cd ipop-rpi_14.01.pre1
    ```

2.  Update config.json with proper credentials

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
    sudo sh -c './ipop-tincan 1> out.log 2> err.log &'
    ```

2.  Lunch the appropriate controller

    a.   For SocialVPN

    ```bash
    ./svpn_controller -c config.json
    ```

    a.   For GroupVPN

    ```bash
    ./gvpn_controller -c config.json
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
    pkill ipop-tincan
    pkill svpn_controller.py
    pkill gvpn_controller.py
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**
