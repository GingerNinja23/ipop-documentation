These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit).

## Download and configure SocialVPN

1.  Download groupvpn and extract for Linux

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-linux_14.01.pre1.tgz
    tar xvzf ipop-linux_14.01.pre1.tgz
    cd ipop-linux_14.01.pre1
    ```

2.  Update the `config.json` file with proper credentials. For GroupVPN it is 
    important to use a different IPv4 address for each machine (e.g.
    192.168.5.1 for machine 1 and 192.168.5.2 for machine 2).

    ```bash
    {
        "ip4": "192.168.5.1",
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com"
    }
    ```

## Running GroupVPN

1.  Launch ipop-tincan

    ```bash
    sudo sh -c './ipop-tincan-x86_64 1> out.log 2> err.log &'
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

**Run groupvpn on another machine using same credentials and they will connect
with each other.**

## Closing GroupVPN

1.  Kill groupvpn

    ```bash
    pkill ipop-tincan-x86_64
    pkill gvpn_controller.py
    ```
