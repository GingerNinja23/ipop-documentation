## Install dependencies

1. Install [MS Visual C++ Redistributable](http://www.microsoft.com/en-us/download/details.aspx?id=40784)

1. Install [Python 2.7 for Windows](http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi).

2. Install [tap-installer from OpenVPN](http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe).

3. Rename the newly installed tap-device to "_ipop_".

    [[1.jpg]]

    [[2.jpg]]

## Install binaries

1. [Download SocialVPN for Windows](https://github.com/ipop-project/downloads/releases/download/v15.01.0/ipop-v15.01.0-x86_win7.zip). 

2. Extract "ipop-15.01.0-x86_win7".

3. Right-click "_config.json_" file and click on "_Edit_". Update with your
   XMPP service credential.  Note: SocialVPN translates IPv4 addresses automatically; you
   should configure each machine with the same IPv4 address (e.g. 172.31.0.100 in the
   configuration file below).
   By default, addresses are assigned by SocialVPN dynamically on a round-robin fashion. 
   Alternatively, you can assign addresses for your peers yourself through an additional 
   configuration file. Please refer to our [[FAQs|FAQs]] for details on how to find out
   or configure the addresses of peers.
   If you don't need address translation, you may consider using GroupVPN instead of SocialVPN.

    <Example>

    ```json
    {
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com",
        "ip4": "172.31.0.100",
        "ip4_mask": 24,
        "stat_report": true,
        "tincan_logging": 0,
        "controller_logging": "DEBUG"
    }
    ```

4. Right-click on "_setup-interface.bat_" file, and click on
    "_Run as administrator_".

    [[3.jpg]]


## Run SocialVPN

1. To start the IPOP Social VPN, double-click on "_start_svpn.bat_" file.Two command shell windows will appear, indicating that it is running. Unless the log level in the config file is changed from the default _INFO_ to _DEBUG_ you will not see the output described below.

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

1. Run SocialVPN on another machine using same credentials and they will
   connect with each other.

   

### Close SocialVPN software
1. Close two console windows.