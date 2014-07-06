1. Prepare XMPP server

   1.1 You can create XMPP server and configure social graph credential automatically as in [Social Graph Depolyment for futuregrid](Social Graph Depolyment for futuregrid) 
  
   1.2 Or you can use preexising XMPP server and deploy XMPP credential with below script.
```
wget https://github.com/ipop-project/ipop-scripts/raw/master/synthesis_graph.py
chmod +x synthesis_graph.sh 
vi synthesis_graph.sh 
# Then change the line g = nx.barabasi_albert_graph(200, 2) as you prefer. 
./synthesis_graph
```
