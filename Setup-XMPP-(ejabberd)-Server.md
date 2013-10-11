1.  Install ejabberd on Ubuntu 12.04

    ```bash
    sudo apt-get update
    sudo apt-get install ejabberd
    ```

2.  Update configuration and restart

    ```bash
    wget -O ejabberd.cfg --no-check-certificate http://goo.gl/iObOjl
    sudo cp ejabberd.cfg /etc/ejabberd/
    sudo service ejabberd restart
    ```

3.  Create default user

    ```bash
    sudo ejabberdctl register svpnuser ejabberd password
    ```

4.  Update svpn_controller.py (or gvpn_controller.py) to point to new STUN server

    ```python
    STUN = "ip-address-of-vm:3478"
    ```

5.  Run socialvpn (or groupvpn) with new credentials

    ```bash
    # this starts socialvpn
    python svpn_controller.py svpnuser@ejabberd password

    # this starts groupvpn
    python gvpn_controller.py svpnuser@ejabberd password ip-address-of-vm
    ```
---

-   You can add more users by going to `http://ip-address-of-vm:5280/admin`
    (click on Virtual Hosts ==\> ejabberd ==\> Users)

    >   username = svpnuser@ejabberd  
    >   password = password

-   If you are running this on the cloud, make sure to open ports

    > TCP - 5222, 5269, 5280  
    > UDP - 3478

---

You can also [[setup a TURN server|Setup TURN Relay]] in cases where your
network firewall does not allow direct connections.