_Disclaimer: currently the Windows implementation tunnels IP, but lacks the key feature DTLS encryption support, and it is not as stable as the Linux implementation. If you are a Windows expert who would like to help with hardening this version, please contact us!_
 
This has been tested on Windows 7 (32bit and 64bit). The libjingle library
currently uses SChannel for Windows which does not have DTLS support 
meaning the connection to the XMPP server is encrypted but
**the P2P VPN connections are not encrypted**.

## Install dependencies

1. Install [MS Visual C++ Redistributable](http://www.microsoft.com/en-us/download/details.aspx?id=5555)

1. Install [Python 2.7 for Windows](http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi).

2. Install [tap-installer from OpenVPN](http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe).

3. Rename the newly installed tap-device to "_ipop_".

    [[1.jpg]]

    [[2.jpg]]

## Install binaries

1. [Download GroupVPN for Windows](http://goo.gl/leX2Qm).

2. Extract "ipop-win32_14.01.zip".

3. You must then edit the "_setup-interface.bat_" and "_config.txt_". Each 
peer that participates in the group VPN must be assigned a unique static IP v4
address. On each node this must be set in two places:
 1) In "_setup-interface.bat_", set your preferred static IP. This will be the 
    IP address for the ipop adapter.
 2) In the "_config.txt_", set the ip4 value to the same address you assigned 
    to the ipop adapter for the current node.
You can also set your xmpp username and password at this point and save the changes
made to both files.

    <Example>

    [[5.jpg]]

4. Right-click on "_setup-interface.bat_" file, and click on
    "_Run as administrator_".

    [[3.jpg]]

    [[4.jpg]]


## Run GroupVPN

1. Double-click on "_start_gvpn.bat_" file whenever you want to run GroupVPN and
   then two command shell windows will show up showing that it is running.

    [[6.jpg]]
    
    * _fpr_ is the X509 fingerprint of the local user
    * _ip4_ is the IPv4 address of the local user
    * _ip6_ is the IPv6 address of the local user
    * _uid_ is the uid of the local user
    * _type_ means this is a local state message

    [[7.jpg]]

    The last 5 lines are the most important lines because they show the node
    connecting to XMPP server and becoming ONLINE. If you do not see this 
    message, that means that you have not properly connected.

2. Run GroupVPN on another machine using same credentials and they will
   connect with each other.

### Close GroupVPN software
1. Close two shell windows.
