These instructions describe how to automate the process of deploying a large social network IPOP overlay on FutureGrid resources using OpenStack nova. We have created scripts that automate the creation of an XMPP server (ejabberd) virtual machine, configuration of ejabberd with a synthetic social graph, as well as virtual machines that run LXC containers, where each container runs an IPOP node.

While the instructions below have been tested on FutureGrid, the same process should work on other OpenStack/nova clouds - all you should need is a baseline Ubuntu 14.04 image.

### 1. Prepare XMPP server

1.1 The simplest way is to create an XMPP server and configure social graph credential automatically with the social_graph.sh script as in [Social Graph Depolyment for futuregrid](Social Graph Depolyment for futuregrid).
```
ssh sierra.futuregrid.org
wget --no-check-certificate https://github.com/ipop-project/ipop-scripts/raw/master/social_graph.sh
chmod +x social_graph.sh
```

  Before running social_graph.sh script, make sure that 1) you have a public key registered with nova, and 2) your public key matched the KEY_NAME variable in the social_graph.sh. Check your keys with:

```
$nova keypair-list
```

  If your public key is not named "public-key", you must change the variable KEY_NAME as your public-key in social_graph.sh file. If you don't have a public key registered at all, you can register it as follows:

```
nova keypair-add --pub_key ~/.ssh/id_rsa.pub public-key
```
  
To automatically deploy an XMPP server with a 300-user social network, simply run:
```
./social_graph.sh -m 1 -n 300
```

If all goes well, you should see output showing the creation of users, and you should see a VM named XMPP in the list of your instances (nova list). If you see "Permission denied (publickey)." messages, then your SSH key is not properly configured; delete the XMPP server (nova delete XMPP), add the keypair as described above, and retry.

 1.2 Alternatively, you can use an existing XMPP server and use the following steps to setup the social network graph in your XMPP server:

```
wget https://pypi.python.org/packages/source/n/networkx/networkx-1.9.tar.gz#md5=683ca697a9ad782cb78b247cbb5b51d6
tar xzvf networkx-1.9.tar.gz

wget -q http://current.cs.ucsb.edu/socialmodels/code/fittingCode.zip
sudo apt-get install unzip
unzip fittingCode.zip
cp fittingCode/socialModels.py .
sudo apt-get install python-setuptools
sudo easy_install decorator
wget https://github.com/ipop-project/ipop-scripts/raw/master/synthesis_graph.py

chmod +x synthesis_graph.sh 
vi synthesis_graph.sh 
# Then change the line g = nx.barabasi_albert_graph(200, 2) as you prefer. 
./synthesis_graph
```

This will subscribe social graph credentials in XMPP and create a file "distance_table". This file shows the distance from one node to the all the other node in terms of shortest distance. For example, {0: {1:[1, 2, 3], 2:[4, 5, 6]}}, means that node 0 has direct peers 1,2 and three and has 4, 5 and 6 as a one hop distance peers(4, 5 and 6 are direct peers of 1, 2 and 3). 

1.3 Check XMPP server

You can check that users have been created on the XMPP server as follows (replace server_ip with the XMPP server's IP address assigned by nova):

```
ssh -l ubuntu server_ip
sudo bash
ejabberdctl registered_users ejabberd
```

### 2. Deploy openstack and lxc instances in futuregrid. 

Make sure you have downloaded the social_graph.sh script (see step 1).

The following command (-m 20 will create 6 openstack instances (-v 6) each containing 50 LXC instances (-l 50), thus in total, 300 LXC instances created. Adjust the parameters, if needed, according to how you configured the XMPP server - you need one LXC instance per XMPP user.  
```
./social_graph.sh -m 2 -v 6 -l 50
```

This script takes a while to run, as it deploys and updates several VM instances. When the script finishes executing, if you run nova list, you should see the XMPP VM (created in step 1), and six (in this example) IPOPx VMs (created in this step). The next step is to run IPOP in all the LXC instances.

### 3. Running IPOP/SocialVPN in all LXC instances

3.1 Going back to your nova client machine (e.g. sierra.futuregrid.org), download the IPOP binary you want to deploy in the system from the [https://github.com/ipop-project/downloads/releases] releases repository. Place executables such as ipop-tincan-x86_64, svpn_controller.py, ipoplib.py and configuration file config.json at your current working directory. In config.json file you should enable multihop mode and icc(inter-controller connection).


```
{
 ...
 "icc": true,
 "multihop": true,
 ...
}
```

3.2 To deploy and run IPOP on all the LXC instances, you now run the social_graph.sh script with -m 3, as follows. 
Note 1: Command-line argument -i: you need to provide a comma-separated list of the IP addresses of all your IPOPx instances deployed in step 2 - you can obtain these IP addresses using the nova list command, then type them following the -i command line argument. 
Note 2: Command-line argument -l: this specifies how many containers per VM (same as step 2)
Note 3: Command-line argument -p: this specifies to run the SocialVPN controller (SVPN)
```
$nova list
./social_graph.sh -m 3 -i "10.35.23.19,10.35.23.20,10.35.23.21,10.35.23.35,10.35.23.36,10.35.23.37" -l 50 -p "SVPN"
```

### 4. Referring distance table for multihop testing
In XMPP server, at the working directory you ran synthesis_graph.sh. You can find file name "distance_table". This file constructs as follows. It means node 0 has direct connection of nodes 10, 20 and 30 and has nodes 12, 13 and 14 as a multihop nodes (It means 12, 13 and 14 are direct peers of 10, 20 and 30)

```
{
 0:{ 1:[10, 20, 30], 2:[12, 13, 14] .. }, 1:{ 1:[5, 6, 7], 2:[15, 16, 17] ..}  ..
}
```

At this point, you should ssh to VM and run below to see the ipv6 of node. For example, when you created 50 LXC instances in a single VM, node 12 LXC instance locates at first VM and the LXC instance name is IPOP12. 
```
$sudo lxc-ls --fancy 
```

Now you can ping from node 0 to node 12 as below command.
```
$ping6 [ipv6 of IPOP12]
```


### 5. Stopping LXC instances 
```
./social_graph.sh -m 4 -i "10.35.23.19,10.35.23.20,10.35.23.21,10.35.23.35,10.35.23.36,10.35.23.37" -l 50
```

