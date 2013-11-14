This has been tested on Windows 7 but it should also work on Windows XP and Windows 8. Also, P2P connections are not encrypted on Windows because libjingle library currently uses SChannel for Windows which does not have DTLS support.

##Install Dependencies

1.  Install tap-installer from OpenVPN (http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe)

2.  1) Go to Control Panel-> Network and Internet -> Network Connections (http://support.microsoft.com/kb/2729523)
    2) Rename the newly installed tap-device to "ipop" 

3.  Open command prompt as administrator and run the command below to configure the IP address of the newly created IPOP interface (http://technet.microsoft.com/en-us/library/cc947813(v=ws.10).aspx) 

    ```bash
    netsh interface ip set address "ipop" static 172.31.0.100 255.255.255.0
    ```

4.  Install Python 2.7 for Windows (http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi)

## Download and run binaries

1.  Download SocialVPN http://goo.gl/fUc4Xa 

2.  Look for a file called "0B8NEUuVLBLpkS0E3VlM3QThTZVk" in your downloads directory and rename "ipop-win32.zip"

3.  Right-click on the file, and click on "Extract All"

4.  Open unzipped folder and double-click on ipop-tincan.exe

5.  Open a Windows command shell (type cmd in search window) and set Python binaries in path and navigate to the folder where you extracted the SocialVPN files.

    ```bash
    set PATH=%PATH%;C:\Python27
    cd <path of extracted socialvp folder>
    ```

6.  Log into XMPP (e.g. Google Chat, Jabber.org, or your own private XMPP server) using username/password credentials and configuring a virtual IP address, in the following example, we are using our Google login credentials

    ```bash
    python svpn_controller.py [username]@gmail.com <password> <talk.google.com>
    ```


**Run socialvpn on another machine using same credentials and they will connect
with each other.**