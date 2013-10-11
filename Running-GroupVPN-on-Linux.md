These instructions are for Ubuntu or Debian (64-bit).

## Download and run binaries

1.  Download groupvpn and extract for Linux

    ```bash
    wget -O ipop.tgz http://goo.gl/cR7CtG
    tar xvzf ipop.tgz
    cd ipop
    ```

2.  Launch groupvpn

    ```bash
    sudo sh -c './ipop-tincan &> log.txt &'
    ```

3.  Log into XMPP (Google Chat or Jabber.org) using credentials

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
