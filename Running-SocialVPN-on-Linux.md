These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit).
Visit the [downloads page ](https://github.com/ipop-project/downloads/releases) to get packages for additional platforms.
## Download and configure SocialVPN

1.  Download SocialVPN and extract for Ubuntu or CentOS

    ```bash
    wget -O ipop-14.07.0_ubuntu12.tar.gz http://goo.gl/IsGzqI
    tar xvzf ipop-14.07.0_ubuntu12.tar.gz
    cd ipop-14.07.0_ubuntu12
    ```

    ```bash
    wget -O ipop-14.07.0-x86_64_CentOS6.tar.gz http://goo.gl/3nHK7Z
    tar xvzf ipop-14.07.0-x86_64_CentOS6.tar.gz
    cd ipop-14.07.0-x86_64_CentOS6
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
        "controller_logging": "DEBUG"
    }
    ```

## Running SocialVPN

1.  First, you need to start the ipop-tincan program

    ```bash
    sudo sh -c './ipop-tincan-x86_64 1> out.log 2> err.log &'
    ```
    Note: use "ipop-tincan-x86" in place of "ipop-tincan-x86_64" for 32-bit Ubuntu machine.
2.  Second, start the SocialVPN controller with the configuration file you created:
   ```bash
    chmod 755 svpn_controller.py
    ```

    ```bash
    ./svpn_controller.py -c config.json &> log.txt &
    ```

3.  Check on the current status of your network. This will show you the IP addresses of other nodes connected to your SocialVPN:

    ```bash
    echo -e '\x02\x01{"m":"get_state"}' | netcat -q 1 -u 127.0.0.1 5800
    ```

    By default, addresses are assigned dynamically on a round-robin fashion. Alternatively, you can assign addresses for your peers yourself through an additional configuration file. Please refer to our [[FAQs|FAQs]] for details.

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

    [[ifconfig.png]]

**Run SocialVPN on another machine using the same credentials and they will connect
with each other.**

## Stopping SocialVPN

1.  Kill SocialVPN 

    ```bash
    pkill ipop-tincan-x86_64
    ps aux | grep svpn_controller.py
    kill <pid-of-svpn-controller.py>
    ```
  Note: use "ipop-tincan-x86" in place of "ipop-tincan-x86_64" for 32-bit Ubuntu machine.
