## Install dependencies

1. Install [MS Visual C++ x86 Redistributable Packages for Visual Studio 2013](http://www.microsoft.com/en-us/download/details.aspx?id=40784)

1. Install [Python 2.7 for Windows](http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi).

2. Install [tap-installer from OpenVPN](http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe).

3. If you are using an XMPP server with a self-signed certificate. Download and install the XMPP server certificate -Right click on the certificate and choose install from the context menu. Chose to _place the certificate in the following store_ and browse to _Trusted Root Certification Authorities_. Click OK and through to finish.

4. Rename the newly installed tap-device to "_ipop_".
From the Control Panel choose _View network status and tasks_ under Network and Internet. On the right side-bar click _change adapter settings_. Select the _TAP Windows Adapter V9_ and rename it to "ipop".

    [[1.jpg]]
    [[2.jpg]]

## Install binaries

1. [Download GroupVPN for Windows](http://goo.gl/sY5yvo).

1. Extract "ipop-14.07.0-x86_win7".

1. You must then edit the  "_config.json_" for each node that participates in the group. At a minimum you must set:
 * A group-wide unique static IPv4 address and the mask.
 * Your XMPP server, username and password.

 ``` json
{
    "xmpp_username": "username@gmail.com",
    "xmpp_host": "talk.google.com",
    "xmpp_password": "enter-password-here",
    "ip4": "172.31.0.100",
    "ip4_mask": 24,
    "controller_logging": "INFO",
    "tincan_logging": 0
    "sec": true, 
}
 ```

1. Right-click on "_setup-interface.bat_" file, and click on
    "_Run as administrator_".

    [[3.jpg]]

    [[4.jpg]]

1. Install IPOP Service by starting a Windows command prompt with administrative privileges. Change your current working directory to the directory containing the IPOP executables and run the following command.
 ```
IPoPSvc --install
 ```

1. To run in the native mode as a Windows service start the service from the Windows Service Control Manager or run the command
 ```
net start IPoPService
 ```

The service, by default, is not configured to start automatically. You are required to manually start the service each time the computer reboots or change the service start mode to Automatic (Delayed Start). This will allow Windows to automatically start the service at the appropriate time whenever the computer is rebooted.
