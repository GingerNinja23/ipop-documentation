These instructions are for Ubuntu 12.04 or higher or Debian Wheezy or later (64-bit).

## Download and run binaries

1.  Download groupvpn/socialvpn and extract for Linux

    ```bash
    wget -O ipop.tgz http://goo.gl/4ecwqn
    tar xvzf ipop.tgz
    cd ipop
    ```

2.  Launch groupvpn

    ```bash
    sudo sh -c './ipop-tincan 1> out.log 2> err.log &'
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
    pkill ipop-tincan
    pkill svpn_controller.py
    pkill gvpn_controller.py
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**