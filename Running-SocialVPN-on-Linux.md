These instructions are for Ubuntu or Debian (64-bit).

## Download and run SocialVPN

1.  Download socialvpn and extract for Linux

    ```bash
    wget -O svpn.tgz http://goo.gl/0LiFiE
    tar xvzf svpn.tgz
    cd svpn
    ```

2.  Launch socialvpn

    ```bash
    sudo sh -c './svpn-jingle 1> out.log 2> err.log &'
    ```

3.  Log into XMPP (Google Chat or Jabber.org) using credentials

    ```bash
    python vpn_controller.py username password xmpp-host
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig svpn
    ```

5.  Kill socialvpn

    ```bash
    pkill svpn-jingle
    pkill vpn_controller.py
    ```

**Run socialvpn on another machine using same credentials and they will connect
with each other.**
