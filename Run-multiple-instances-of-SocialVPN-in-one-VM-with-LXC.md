## THIS PAGE IS OUTDATED

These instructions will explain how to run multiple versions of SocialVPN on one
VM using LXC.

1.  Download our pre-made script

    ```bash
    wget https://github.com/ipop-project/ipop-scripts/raw/master/svpn_lxc.sh
    ```

2.  Run the script and wait, the following command will create 5 containers
    (named container1, container2, ..., container5) each running an instance of
    socialvpn


    ```bash
    sudo sh -c "sh svpn_lxc.sh username password host 1 5 30 svpn \
                1> out.log 2> err.log &"
    ```

3.  Check to see if containers are running

    ```bash
    ps aux | grep lxc-start
    ```

4.  Get console access to container1 (username: `ubuntu`, password: `ubuntu`)

    ```bash
    sudo lxc-console -n container1
    ```

5.  You can check to see if socialvpn is running with the following commands

    ```bash
    ps aux | grep svpn-jingle
    ps aux | grep controller.py
    /sbin/ifconfig
    ping 172.31.0.101
    ```

6.  You can exit out of a container with <kbd>ctrl</kbd>+<kbd>a</kbd>,
    <kbd>q</kbd> then shutdown the container

    ```bash
    sudo lxc-shutdown -n container1
    ```
