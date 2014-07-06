1. Prepare XMPP server

   1.1 You can create XMPP server and configure social graph credential automatically as in [Social Graph Depolyment for futuregrid](Social Graph Depolyment for futuregrid) 
  
   1.2 Or you can use preexising XMPP server and deploy XMPP credential with below script.
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
This will create the social graph credential and 