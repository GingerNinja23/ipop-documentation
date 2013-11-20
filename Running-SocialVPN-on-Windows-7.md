This has been tested on Windows 7 but it should also work on Windows XP and Windows 8. Also, P2P connections are not encrypted on Windows because libjingle library currently uses SChannel for Windows which does not have DTLS support.

##Install Dependencies

1.  Install tap-installer from OpenVPN http://swupdate.openvpn.org/community/releases/tap-windows-9.9.2_3.exe

2.  Rename the newly installed device "ipop" http://support.microsoft.com/kb/2729523

3.  Install Python 2.7 for Windows http://www.python.org/ftp/python/2.7.5/python-2.7.5.msi

## Install binaries

1.  Download our software from http://goo.gl/F5TF1Y

2.  Look for a file called "0B8NEUuVLBLpkaGFlS25Nck5NZ2M" in your downloads directory and rename "ipop-win32.zip"

3.  Right-click on the file, and click on "Extract All"

3.  Right-click on setup-interface file, and click on "Run as administrator"

4.  Open config.txt with Notepad and update with your Google username and password

## Run SocialVPN

1.  Double-click on start file and two command shell windows will show up showing that it is running

**Run socialvpn on another machine using same credentials and they will connect
with each other.**