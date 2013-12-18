These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit).

## Download and configure SocialVPN

1.  Download socialvpn and extract for Linux

    ```bash
    wget http://www.acis.ufl.edu/~ptony82/ipop/ipop-linux_14.01.pre3.tgz
    tar xvzf ipop-linux_14.01.pre3.tgz
    cd ipop-linux_14.01.pre3
    ```

2.  Update the `config.json` file with proper credentials. For SocialVPN, you
    don't have to change the *ip4* address.

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

    ```bash
    ./svpn_controller.py -c config.json &> log.txt &
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

**Run socialvpn on another machine using same credentials and they will connect
with each other.**

## Closing SocialVPN

1.  Kill socialvpn 

    ```bash
    pkill ipop-tincan-x86_64
    pkill svpn_controller.py
    ```
