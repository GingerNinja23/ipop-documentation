These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit).

## Download and configure SocialVPN

1.  Download groupvpn/socialvpn and extract for Linux

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-linux_14.01.pre1.tgz
    tar xvzf ipop-linux_14.01.pre1.tgz
    cd ipop-linux_14.01.pre1
    ```

2.  Update the `config.json` file with proper credentials (see below)

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
    sudo sh -c './ipop-tincan-x86_64 1> out.log 2> err.log &'
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
    pkill ipop-tincan-x86_64
    pkill svpn_controller.py
    pkill gvpn_controller.py
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**
