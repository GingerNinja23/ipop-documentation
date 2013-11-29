These instructions have only been tested on Ubuntu 12.04

## Install and configure ejabberd

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

## Configure SocialVPN/GroupVPN

1.  Update config.json file to point to new STUN
    server, we recommend that you use the public IP address (not DNS) of the 
    STUN server, especially if server is running on a public cloud (i.e. 
    Amazon EC2)

    ```bash
    {
        "ip4": "172.31.0.100",
        "xmpp_username": "user",
        "xmpp_password": "blah",
        "xmpp_host": "example.com",
        "stun": ["public-ip-of-your-vm:3478"],
    }
    ```

## Additional ejabberd configurations

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
