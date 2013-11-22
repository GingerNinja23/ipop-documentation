These instructions are for Ubuntu 12.04 or higher or Debian Wheezy or later (64-bit).

## Download and run binaries

1.  Download groupvpn/socialvpn and extract for Linux

    ```bash
    wget -O ipop.tgz http://goo.gl/4ecwqn
    tar xvzf ipop.tgz
    cd ipop
    ```

2.  Update config file with proper credentials

    ```bash
    vi config.json
    ```

3.  Launch groupvpn

    ```bash
    sudo sh -c './ipop-tincan 1> out.log 2> err.log &'
    ```

4.  Start the appropriate controller

    a.   For SocialVPN

    ```bash
    ./svpn_controller.py -c config.json
    ```

    a.   For GroupVPN

    ```bash
    ./gvpn_controller.py -c config.json
    ```

5.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

6.  Kill socialvpn or groupvpn

    ```bash
    pkill ipop-tincan
    pkill svpn_controller.py
    pkill gvpn_controller.py
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**
