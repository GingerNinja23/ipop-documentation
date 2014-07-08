# 1. Prepare XMPP server

   1.1 You can create XMPP server and configure social graph credential automatically as in [Social Graph Depolyment for futuregrid](Social Graph Depolyment for futuregrid) 
```
wget https://github.com/ipop-project/ipop-scripts/raw/master/social_graph.sh
```
  
   1.2 Or you can use preexising XMPP server and deploy XMPP credential as below. 
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

# 2. Deploy openstack and lxc instances in futuregrid. 

Below command will create 6 openstack instances each containing 50 LXC instances, thus in total, 300 LXC instances created. 
```
./social_graph.sh -m 2 -v 6 -l 50
```
