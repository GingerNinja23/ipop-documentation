These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit).

## Download and configure GroupVPN

1.  Download groupvpn and extract for Linux

    ```bash
    wget wget -O ipop-linux_14.01.tgz http://goo.gl/fZSLhY
    tar xvzf ipop-linux_14.01.tgz
    cd ipop-linux_14.01
    ```

2.  Update the `config.json` file with proper XMPP server address, and the
    user name and password of the XMPP user. You can use existing public XMPP services,
    or you can also [[setup your own XMPP server|Installing XMPP Server]].
    GroupVPN currently supports static
    IPv4 address assignment - you must configure a different IPv4 address for each 
    machine (e.g. 192.168.5.1 for machine 1 and 192.168.5.2 for machine 2).

    ```bash
    {
        "xmpp_username": "username@gmail.com",
        "xmpp_password": "enter-password-here",
        "xmpp_host": "talk.google.com",
        "ip4": "192.168.5.1",
        "ip4_mask": 24,
        "tincan_logging": 0,
        "controller_logging": "logging.INFO"
    }
    ```

## Running GroupVPN

1.  First, launch the ipop-tincan program

    ```bash
    sudo sh -c './ipop-tincan-x86_64 1> out.log 2> err.log &'
    ```

2.  Second, start the GroupVPN controller

    ```bash
    ./gvpn_controller.py -c config.json &> log.txt &
    ```

3.  Check on the current status of your network using netcat

    ```bash
    echo '{"m":"get_state"}' | netcat -u 127.0.0.1 5800
    ```

4.  Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

    [[ifconfig.png]]

## Stopping GroupVPN

1.  Kill groupvpn

    ```bash
    pkill ipop-tincan-x86_64
    pkill gvpn_controller.py
    ```

**Run groupvpn on another machine using same credentials and they will connect
with each other.**

## Running GroupVPN in Watchdog mode

It is common practice to use a watchdog process to monitor and respawn
long running processes. We have designed a simple watchdog process that
spawns ipop-tincan and respawns it up to three times if necessary.

Our watchdog process automatically starts the ipop-tincan, so that you 
do not have to run it separately. (the path of the binary should be specified
in configuration file). If the ipop-tincan crashes or is not responding, 
the watchdog process terminates the ipop-tincan process and starts it as a 
new process.

```
sudo ./watchdog.py -c config.json
```
## On-demand GroupVPN Connection mode

GroupVPN has two modes on establishing the P2P connection. One create P2P connection once it starts to run, the other starts to establish connection when there appears a packet that is destined to a node without P2P connection yet. We call former as proactive mode and latter as on-demand mode. 

### Proactive Connection Mode
 Proactive connection establishes connection to all remote nodes right after it starts to run. The established connections are persistent given that the GroupVPN is running. It is the default mdoe of operation, but has a drawback of connection overhead when the node number increases. Note that when new node appears on GroupVPN, it establishes connections to all nodes in the GroupVPN network. 

### On-demand Connection Mode
 On-demand connection mode establishes P2P connection only when there is a demand on connection. Technically, when a packet that is destined to a node without P2P connection yet is captured in a tap device, it starts to establish P2P connection. This mode of connection is useful for reducing connection overhead. It also disconnects P2P connection after given threshold of period without traffic.  On-demand connection is configurable through config file. Below two fields are relevant to on-demand connection mode. 

```bash
{
    "on-demand_connection": true,
    "on-demand_inactive_timeout": 600
}
```

Set the "on-demand_connection" field to true allows on-demand connection. Omitting or setting false this field makes the GroupVPN run on default(proactive) mode. "On-demand_inactive_timeout" sets the threshold period of disconnecting P2P. 

