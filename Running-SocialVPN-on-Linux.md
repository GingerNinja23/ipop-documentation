These instructions are for Ubuntu or Debian (64-bit).

## Download and run binaries

1.  Download socialvpn and extract for Linux

    ```bash
    wget -O ipop.tgz http://goo.gl/4ecwqn
    tar xvzf ipop.tgz
    cd ipop
    ```

2.  Launch socialvpn

    ```bash
    sudo sh -c './ipop-tincan 1> out.log 2> err.log &'
    ```

3.  Log into XMPP (Google Chat or Jabber.org) using credentials

    ```bash
    python svpn_controller.py username password xmpp-host
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

5.  Kill socialvpn

    ```bash
    pkill ipop-tincan
    pkill svpn_controller.py
    ```

**Run socialvpn on another machine using same credentials and they will connect
with each other.**