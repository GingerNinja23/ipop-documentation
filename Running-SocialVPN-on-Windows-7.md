This has been tested on Windows 7 only. Also, P2P connections are not encrypted on Windows because libjingle library currently uses SChannel for Windows which does not have DTLS support.

### Install Dependencies

1. Install _Python 2.7_ for _Windows_ from http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi.

1. Install _tap-installer_ from OpenVPN (http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe).

1. Go to _Control Panel_->_Network and Internet_->_Network Connections_ and then rename the newly installed tap-device to "_ipop_" (http://support.microsoft.com/kb/2729523).

    [[1.jpg]]

    [[2.jpg]]

### Install binaries

1. Download our software version 0.1 from http://www.acis.ufl.edu/~ptony82/ipop/ipop-win32_0.1.zip.

1. Extract "_ipop-win32_0.1.zip_".

1. Right-click on "_setup-interface.bat_" file, and click on "_Run as administrator_".

    [[3.jpg]]

    [[4.jpg]]

1. Right-click "_config.txt_" file and click on "_Edit_". Update with your XMPP service credential. 
<Example>

    [[5.jpg]]

### Run SocialVPN/GroupVPN software

1. Double-click on "_start.bat_" file whenever a user wants to run SocialVPN/GroupVPN software and then two command shell windows will show up showing that it is running.

    [[6.jpg]]
    
    * The first line shows the configurations that the controller will use to run.
    * _fpr_ is the X509 fingerprint of the local user
    * _ip4_ is the IPv4 address of the local user
    * _ip6_ is the IPv6 address of the local user
    * _uid_ is the uid of the local user
    * _type_ means this is a local state message

    [[7.jpg]]

    The last 5 lines are the most important lines because they show the node connecting to XMPP server and becoming ONLINE. If you do not see this message, that means that you have not properly connected.

1. Run socialvpn on another machine using same credentials and they will connect with each other.

### Close SocialVPN/GroupVPN software
1. Close two shell windows.