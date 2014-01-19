This tutorial shows the deployment of Social VPN in a form of Social Network Graph. Also, part of this tutorial shows how to deploy multiple VMs with LXC instances running IPOP tincan and controllers, which enables more than hundred or even thousand scale of nodes deployment. 

1 - Download the social_graph.sh script and take a look at info. 

```bash
wget https://github.com/ipop-project/ipop-scripts/raw/master/social_graph.sh
chmod +x ./social_graph.sh
./social_graph.sh
```
2 - Below command will create 300 nodes with random friends list. 

```bash
./social_graph.sh -m 1 -n 300
```
If you want to configure the social graph itself, you can login to created xmpp instance and take a look "synthesis_graph.py".

3 - Below command will create 6 VMs and each VM contains 50 LXC instances. Creating VMs are done in a sequential manner. But creating LXCs are done in parallel to reduce time load, so that each VMs are creating LXC instances at the same time. The stdout is messy and there is a possibility that few of created VMs are not functioning well. You may want to ping test to each VMs. 

```bash
./social_graph.sh -m 2 -v 6 -l 50
nova list
ping 
```
4 - Below command will copy IPOP executables and config file to each LXC instance, then run it. Also, it  configure each username or IP Address accordingly. Note that, you should provide the list of IP address of each VMs. 

```bash
./social_graph.sh -m 3 -i "10.35.23.165,10.35.23.178" -l 50 -p "SVPN"
```
5 - If you want to stop the depolyed lxc instances. Below command helps.

```bash
./social_graph.sh -m 4 -i "10.35.23.165,10.35.23.178" -l 50
```