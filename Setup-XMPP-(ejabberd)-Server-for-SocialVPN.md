1. Installing ejabberd on Ubuntu 12.04

    sudo apt-get install ejabberd

2. Create default user

    sudo ejabberdctl svpnuser ejabberd password

3. Update configuration and restart

    wget -O ejabberd.cfg http://goo.gl/Oc3pQi
    cp ejabberd.cfg /etc/ejabberd/
    sudo service ejabberd restart

4. You can add more users by going to http://ip-address-of-vm:5280/admin

* username = svpnuser
* password = password

6. (Optional) If you are running this on the cloud, make to open ports

* TCP - 5222, 5269, 5280
* UDP - 3478