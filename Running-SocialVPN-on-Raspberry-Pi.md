These instructions are for Raspbian (Raspberry Pi) compiled with vfp and hard
floating point.

## Download and configure SocialVPN

1.  Download socialvpn and extract for Raspberry Pi

    ```bash
    wget -O ipop-14.01.1-arm_raspbian.tar.gz http://goo.gl/FpMNjC
    tar xvzf ipop-14.01.1-arm_raspbian.tar.gz
    cd ipop-ipop-14.01.1-arm_raspbian
    ```

2.  Update the `config.json` file with proper credentials. For SocialVPN, you
    don't have to change the *ip4* address.


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

## Running SocialVPN

1.  Launch ipop-tincan

    ```bash
    sudo sh -c './ipop-tincan 1> out.log 2> err.log &'
    ```

2.  Start the appropriate controller

    ```bash
    ./svpn_controller.py -c config.json &> log.txt &
    ```

3.  Check on the current status of your network using netcat

    ```bash
    echo '{"m":"get_state"}' | netcat -u 127.0.0.1 5800
    ```
    By default, addresses are assigned dynamically on a round-robin fashion. Alternatively, you can assign addresses for your peers yourself through an additional configuration file. Please refer to our [[FAQs|FAQs]] for details.

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

    [[ifconfig.png]]

## Closing SocialVPN

1.  Kill socialvpn 

    ```bash
    pkill ipop-tincan
    ps aux | grep svpn_controller.py
    kill <pid-of-svpn-controller.py>
    ```

**Run socialvpn on another machine using same credentials and they will connect
with each other.**
