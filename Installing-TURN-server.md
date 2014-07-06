Sometimes SocialVPN nodes cannot connect because of crazy NATs (i.e. symmetric)
or strict firewalls. In such cases, we rely on relaying. This page shows you how
to run a TURN relay in the cloud. We are currently using the following [TURN
implementation](http://turnserver.sourceforge.net). TURN servers require a public 
IP address.

These instructions have only been tested on Ubuntu 12.04 (64-bit).

## Download TURN server

1.  Install TURN implementation dependencies

    ```bash
    sudo apt-get update
    sudo apt-get install libconfuse0
    ```

2.  If you are running on the cloud (e.g. EC2), you need to use IP aliasing to
    allow the TURN server to bind your public IP address

    ```bash
    sudo ifconfig eth0:0 <public-ip-of-vm> up
    ```

3.  Download and extract pre-packaged TURN binaries

    ```bash
    wget -O turn.tgz http://goo.gl/EMzy9Z
    tar xzvf turn.tgz
    ```

## Configure and run TURN server

1.  Update the TURN config file with the public IP address

    ```bash
    sed -i 's/listen_address = .*/listen_address = { "public-ip-address" }/g' turn/turn.conf
    ```

2.  (Optional) Increase file descriptor limit to allow for thousands of TURN connections by
    adding following to `/etc/security/limits.conf` file

    ```
    ubuntu    hard    nofile    100000
    ubuntu    soft    nofile    100000
    ```

3.  Run the TURN server

    ```bash
    cd turn; ./turnserver -c turn.conf; cd ..
    ```

4.  Verify TURN server is running

    ```bash
    netstat -aupn | grep 19302
    ```

## Configure SocialVPN/GroupVPN 

1.  Update your config.json file with your TURN settings, see below for template 

    ```
    {
        "ip4": "172.31.0.100",
        "xmpp_username": "ipopuser@ejabberd",
        "xmpp_password": "password",
        "xmpp_host": "public-ip-of-ejabberd-server",
        "stun": ["public-ip-of-your-vm:19302"],
        "turn": [
            {"server": "public-ip-of-your-vm:19302", "user": "svpnjingle", "pass": "1234567890"}
        ]
    }
    ```

