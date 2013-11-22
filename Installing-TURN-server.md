Sometimes SocialVPN nodes cannot connect because of crazy NATs (i.e. symmetric)
or strict firewalls. In such cases, we rely on relaying. This page shows you how
to run a TURN relay in the cloud. We are currently using the following TURN
implementation. TURN servers require a public IP address. These instructions
have only been tested on Ubuntu 12.04 (64-bit).

http://turnserver.sourceforge.net/

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

4.  Update the TURN config file with the public IP address

    ```bash
    sed -i 's/listen_address = .*/listen_address = { "public-ip-address" }/g' \
           turn/turn.conf
    ```
5.  Run the TURN server

    ```bash
    sudo bash; ulimit -n 16000
    cd turn; ./turnserver -c turn.conf; cd ..
    ```

6.  Verify TURN server is running

    ```bash
    netstat -aupn | grep 19302
    ```

7.  Update your config.json file with your TURN settings

```bash
{
    "ip4": "172.31.0.100",
    "xmpp_username": "user",
    "xmpp_password": "blah",
    "xmpp_host": "example.com",
    "stun": ["public-ip-of-your-vm:19302"],
    "turn": [
        {"server": "public-ip-of-your-vm:19302", "user": "bob", "pass": "apple"}
    ],
}
```

