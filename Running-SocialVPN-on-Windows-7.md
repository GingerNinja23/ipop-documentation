_Disclaimer: SocialVPN release 14.01.1 (and below) do not provide encryption for P2P connections. P2P encryption is available on release 14.01.2 and above._

## Install dependencies

1. Install [MS Visual C++ Redistributable](http://www.microsoft.com/en-us/download/details.aspx?id=40784)

1. Install [Python 2.7 for Windows](http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi).

2. Install [tap-installer from OpenVPN](http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe).

3. Rename the newly installed tap-device to "_ipop_".

    [[1.jpg]]

    [[2.jpg]]

## Install binaries

1. [Download SocialVPN for Windows](http://goo.gl/t7HxnU).

2. Extract "ipop-14.07.0-x86_win7".

3. Right-click "_config.txt_" file and click on "_Edit_". Update with your
   XMPP service credential.  For SocialVPN, you
   don't have to change the *ip4* address.

    <Example>

    [[5.jpg]]

    ```bash
    {
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com",
        "ip4": "172.31.0.100",
        "ip4_mask": 24,
        "tincan_logging": 0,
        "controller_logging": "DEBUG"
    }
    ```


4. Right-click on "_setup-interface.bat_" file, and click on
    "_Run as administrator_".

    [[3.jpg]]


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

2. Run SocialVPN on another machine using same credentials and they will
   connect with each other.

    By default, addresses are assigned dynamically on a round-robin fashion. Alternatively, you can assign addresses for your peers yourself through an additional configuration file. Please refer to our [[FAQs|FAQs]] for details.

### Close SocialVPN software
1. Close two shell windows.
