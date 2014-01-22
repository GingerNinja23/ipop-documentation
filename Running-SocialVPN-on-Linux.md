These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit).

## Download and configure SocialVPN

1.  Download socialvpn and extract for Linux

    ```bash
    wget http://goo.gl/GEG3RP
    tar xvzf ipop-linux_14.01.tgz
    cd ipop-linux_14.01
    ```

2.  Update the `config.json` file with the XMPP server address, and the user name
    and password. You don't need to change the *ip4* address.

    ```bash
    {
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com",
        "ip4": "172.31.0.100",
        "ip4_mask": 24,
        "tincan_logging": 0,
        "controller_logging": "logging.INFO"
    }
    ```

## Running SocialVPN

1.  First, you need to start the ipop-tincan program

    ```bash
    sudo sh -c './ipop-tincan-x86_64 1> out.log 2> err.log &'
    ```

2.  Second, start the SocialVPN controller with the configuration file you created:

    ```bash
    ./svpn_controller.py -c config.json &> log.txt &
    ```

3.  Check on the current status of your network. This will show you the IP addresses of other nodes connected to your SocialVPN:

    ```bash
    echo '{"m":"get_state"}' | netcat -u 127.0.0.1 5800
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

    [[ifconfig.png]]

**Run socialvpn on another machine using the same credentials and they will connect
with each other.**

## Stopping SocialVPN

1.  Kill socialvpn 

    ```bash
    pkill ipop-tincan-x86_64
    pkill svpn_controller.py
    ```
