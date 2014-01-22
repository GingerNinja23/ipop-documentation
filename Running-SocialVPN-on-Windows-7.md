_Disclaimer: currently the Windows port should be consider a demonstration - it tunnels IP, but lacks a key feature (DTLS encryption support) and it is not as stable as the Linux port which is our primary target. If you are a Windows expert who would like to help with hardening this version, please contact us!_
 
This has been tested on Windows 7 (32bit and 64bit). The libjingle library
currently uses SChannel for Windows which does not have DTLS support 
meaning the connection to the XMPP server is encrypted but
**the P2P VPN connections are not encrypted**.

## Install dependencies

1. Install [Python 2.7 for Windows](http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi).

2. Install [tap-installer from OpenVPN](http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe).

3. Rename the newly installed tap-device to "_ipop_".

    [[1.jpg]]

    [[2.jpg]]

## Install binaries

1. [Download SocialVPN for Windows](http://goo.gl/Iw4OMZ).

2. Extract "ipop-win32_14.01.zip".

3. Right-click on "_setup-interface.bat_" file, and click on
    "_Run as administrator_".

    [[3.jpg]]

    [[4.jpg]]

4. Right-click "_config.txt_" file and click on "_Edit_". Update with your
   XMPP service credential.  For SocialVPN, you
   don't have to change the *ip4* address.

    <Example>

    [[5.jpg]]

## Run SocialVPN

1. Double-click on "_start.bat_" file whenever you want to run SocialVPN and
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

2. Run socialvpn on another machine using same credentials and they will
   connect with each other.

### Close SocialVPN software
1. Close two shell windows.
