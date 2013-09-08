### Install ejabberd on Ubuntu 12.04

    sudo apt-get install ejabberd

### Update configuration and restart

    wget -O ejabberd.cfg http://goo.gl/Oc3pQi
    cp ejabberd.cfg /etc/ejabberd/
    sudo service ejabberd restart

### Create default user

    sudo ejabberdctl register svpnuser ejabberd password

### You can add more users by going to http://ip-address-of-vm:5280/admin

* username = svpnuser
* password = password

### (Optional) If you are running this on the cloud, make sure to open ports

* TCP - 5222, 5269, 5280
* UDP - 3478

### Update vpn_controller.py (line 15) to point to new STUN server

    STUN = "ip-address-of-vm:3478"

### Run socialvpn with new credentials

    python vpn_controller.py svpnuser@ejabberd password <ip-address-of-vm>

ejabberd limits the number of sessions per user to 10, you can change this with the _max_user_sessions_ parameter in the ejabberd configuration file.

You can also setup a TURN server in cases where your network firewall does not allow direct connections. I recommend this implementation http://turnserver.sourceforge.net/index.php?n=Main.HomePage