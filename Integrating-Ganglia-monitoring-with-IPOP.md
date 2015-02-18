##  Introduction

Ganglia is a open source monitoring tool that can be used to collect system statistics in a distributed systems environment eg- collecting information from all compute nodes in a cluster, more details can be found by following the below link-  

[ganglia home page](http://ganglia.sourceforge.net/)

To leverage features provided by ganglia, we have written a python ganglia module that could be used to report IPOP specific statistics to a master monitoring node. As of now this module reports back the following stats.  
1. Number of active links.  
2. Incoming traffic/link. (bytes/sec)  
3. Outgoing traffic/link. (bytes/sec)  
4. Link status ON/OFF  
5. Age per link (secs)  
6. Peer XMPP time (secs)  
The module is extensible and can be modified to report additional statistics.  
  
## Installation  
The below instructions assume that the reader is already familiar with ganglia setup,if not below link might help.  
[Ganglia on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/introduction-to-ganglia-on-ubuntu-14-04)  
There are two components to any python ganglia module, for ours we have --  
1. The python module itself-- ipop_ganglia.py.  
2. Configuration file for the module-- ipop_ganglia.pyconf  

Before we install the module we will have to setup the path from which the module will be reading the statistics, to do this we will have to set "net_stats_file" variable in the module source code, this should match with the path specified in the controller while enabling ganglia monitoring.  
```python 
# Where to get the stats from
net_stats_file = "/home/user/ipop-14.07.01_ubuntu14/"
```  
rest of the process is generic and can be found in the below links, note we can set the environment to run on top of IPOP-overlay network-- just use IPOP IP's when configuring monitoring and master nodes.  
  
[python ganglia modules](https://github.com/ganglia/monitor-core/wiki/Ganglia-GMond-Python-Modules)  
[Blog- setting up python ganglia modules](https://sachinsharm.wordpress.com/2013/08/19/setup-and-configure-ganglia-python-modules-on-centosrhel-6-3/)
