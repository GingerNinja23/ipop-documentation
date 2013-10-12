These instructions are for Ubuntu or Debian (64-bit).

## Download and run binaries

1.  Download groupvpn and extract for Linux

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

    ```bash
    python gvpn_controller.py username password xmpp-host ip-address
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

5.  Kill groupvpn

    ```bash
    pkill ipop-tincan
    pkill gvpn_controller.py
    ```

**Run groupvpn on another machine using same credentials and they will connect
with each other.**