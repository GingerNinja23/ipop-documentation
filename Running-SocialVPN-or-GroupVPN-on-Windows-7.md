This has been tested on Windows 7 (32-bit) but it should also work on Win7 64bits (and Windows XP) but I'm not too sure about Windows 8. It may work as well. Also, P2P connections are not encrypted on Windows because libjingle library currently uses SChannel for Windows which does not have DTLS support (but we will fix that soon).

##Install Dependencies

1.  Install tap-installer from OpenVPN (http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe) - Choose 'Virtual Ethernet Interface' option. 

2.  1) Go to Control Panel-> Network and Internet -> Network Connections (http://support.microsoft.com/kb/2729523)
    2) Rename the newly installed tap-device to "ipop" 

3.  Configure tap device with static ip address

    ```bash
    netsh interface ip set address "ipop" static 172.31.0.100 255.255.255.0
    ```

4.  Install Python 2.7 for Windows (http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi)

## Download and run binaries

1.  Download groupvpn/socialvpn http://goo.gl/fUc4Xa and unzip

2.  Open unzipped folder and double-click on ipop-tincan.exe

3.  Open a Windows command shell (type cmd in search window) and set Python binaries in path and navigate to folder

    ```bash
    set PATH=%PATH%;C:\Python27
    cd Downloads\ipop-win32
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


**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**