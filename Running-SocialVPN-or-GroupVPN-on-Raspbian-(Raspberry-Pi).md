These instructions are for Raspbian (Raspberry Pi) compiled with vfp and hard floating point.

## Download and run binaries

1.  Download groupvpn/socialvpn and extract for Linux

    ```bash
    wget -O ipop-rpi.tgz http://www.acis.ufl.edu/~ptony82/ipop/ipop-rpi.tgz
    tar xvzf ipop-rpi.tgz
    cd ipop
    ```

2.  Update config.json with proper credentials

    ```bash
    vi config.json
    ```

3.  Launch ipop-tincan

    ```bash
    sudo sh -c './ipop-tincan-rpi 1> out.log 2> err.log &'
    ```

4.  Lunch the appropriate controller

    a.   For SocialVPN

    ```bash
    ./svpn_controller -c config.json
    ```

    a.   For GroupVPN

    ```bash
    ./gvpn_controller -c config.json
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

5.  Kill socialvpn or groupvpn

    ```bash
    pkill ipop-tincan-rpi
    pkill svpn_controller.py
    pkill gvpn_controller.py
    ```

**Run groupvpn/socialvpn on another machine using same credentials and they will connect
with each other.**
