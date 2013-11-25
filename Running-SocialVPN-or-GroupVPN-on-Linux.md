These instructions are for Ubuntu 12.04 or higher or Debian Wheezy or later (64-bit).

### Download and configure SocialVPN

1.  Download groupvpn/socialvpn and extract for Linux

    ```bash
    wget -O ipop-linux_0.1.tgz http://www.acis.ufl.edu/~ptony82/ipop/ipop-linux_0.1.tgz
    tar xvzf ipop-linux_0.1.tgz
    cd ipop-linux_0.1
    ```

2.  Update config file with proper credentials

    ```bash
    vi config.json
    ```

### Running SocialVPN

1.  Launch ipop-tincan

    ```bash
    sudo sh -c './ipop-tincan 1> out.log 2> err.log &'
    ```

2.  Start the appropriate controller

    a.   For SocialVPN

    ```bash
    ./svpn_controller.py -c config.json
    ```

    a.   For GroupVPN

    ```bash
    ./gvpn_controller.py -c config.json
    ```

3.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

### Closing SocialVPN

1.  Kill socialvpn or groupvpn

    ```bash
    pkill ipop-tincan
    pkill svpn_controller.py
    pkill gvpn_controller.py
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**
