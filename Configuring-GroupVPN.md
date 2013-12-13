
GroupVPN has two modes on establishing the P2P connection. One create P2P connection once it starts to run, the other starts to establish connection when there appears a packet that is destined to a node without P2P connection yet. We call former as persistent connection and latter as on-demand connection. 

- Persistent Connection Mode
 Persistent connection establishes connection to all remote nodes right after it starts to run. The established connections are persistent given that the GroupVPN is running. It is the default mdoe of operation, but has a drawback of connection overhead when the node number increases. Note that when one node appears on GroupVPN, it establishes connections to all nodes in the GroupVPN network. 

- On-demand Connection Mode
 On-demand connection mode establishes P2P connection only when there is a demand on connection. Technically, when a packet that is destined to a node without P2P connection yet is captured in a tap device, it starts to establish P2P connection. This mode of connection is useful for reducing connection overhead 
 It also disconnects P2P connection after given threshold of period of no traffic. 
 On-demand connection is configurable through config file. Below fields 

   "on-demand_connection": true,
   "on-demand_inactive_timeout": 600
 




- 