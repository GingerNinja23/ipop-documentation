These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit). Visit the downloads page to get packages for additional platforms


### Overview of adminGVPN
The idea of adminGVPN takes inspiration from a chat room (MuC) environment, where users otherwise unrelated (not necessarily friends) come together to discuss a topic of common interest and subsequently leave when done.  
  
It is worthwhile to make note of some salient features provided by this environment.   
1. Notification broadcast of the availability of other members (on-line,off-line status).  
2. Notification broadcast when a new member joins or an existing member leaves.  
3. Broadcast of messages to all members of the room.  
4. Access control to the room by a designated administrator, who most often is the member who created the room.  
  
A analogy can be drawn between "discussing a common issue" and "collaborating on a common task", such as for example sharing computing resources etc. This kind of usage scenario can definitely take advantage of a Virtual Private Network (VPN) that would make make it possible for users to run applications on top of IP layer provided by this VPN and thus enabling them to interact seamlessly even when they do not have publicly addressable IP's.  


This VPN connectivity is provided by IPOP which leverages the XEP-0045 (MuC) features provided by XMPP protocol to bootstrap connections.  adminGVPN differs from GVPN in the sense that it lets users to create a overlay network without establishing permanent social relationships along with ease of administration in terms of setting up a network.In practice this provides below advantages  
1. In a group with 'n' members, to include a new members you don't have to execute '2n' instructions/steps to add a new member or remove an existing member.  
2. Allocation of unique IPv4 IPOP addresses is easier as they are handled/revoked by a single admin using an automated script.  

  Let us go through a usage scenario to elaborate how adminGVPN can be utilized. Suppose five people Alice, Bob, Carol, Tom and Mike want to set up a grid computing cluster to share their computing resources to run a job. Let us assume all of them have a social network account supporting XMPP protocol and they designate Alice as the administrator of the group. The steps below detail how the group would than proceed to establish the network.  
 
1. Alice should make use of a separate JID (XMPP account) for administration and for her local machine which she wants to be connected to the IPOP network .
2. Alice creates a new MuC room for the task on her XMPP server  by executing script create_room.py with her admin account JID.  
3. All members of the Group start adminGVPN on their local machines.  
4. Alice sends invitation to herself(XMPP JID for adminGVPN), Bob, Carol, Tom and Mike to join the room by executing script manageUsers.py with her admin account JID, the invitation message contains unique Ipv4 IPOP address for each of them.
5. adminGVPN running on the local machines of Alice, Bob, Carol, Tom and Mike will than automatically accept the invitation, extract the IPv4 IPOP address configure IPOP and trigger the IPOP link creation process.  
6. After this step IPOP network should be established among them.
7. If Alice wants to remove Mike from the room and adminGVPN network, she executes manageUsers.py , this time with delete argument to block his access to the room and asks others to restart adminGVPN.  

![](https://cloud.githubusercontent.com/assets/7332136/5923750/cc693332-a623-11e4-800c-dc470bbed310.JPG) 

 
  
in terms of implementation unlike SVPN, GVPN where XMPP functionality is embedded in libjingle, XMPP interaction is handled in the controller which allows more flexibility to modify/tailor the bootstrap process. 
  
### Prerequisites
1. access to a XMPP service/server with rights to create/configure/manage MuC rooms.
2. each participating entity must have a unique JID and nickname.

### Download and install dependency package.

To abstract XMPP interaction in the controller adminGVPN depends on SleekXMPP, sleekXMPP can be installed easily by following the below steps.
```bash
    sudo apt-get install python-pip
    sudo pip install sleekxmpp
```

### Download admin_gvpn package

  Download admin_gvpn and extract for Ubuntu or CentOS

```bash
 wget -O ipop-14.07.0_ubuntu12.tar.gz http://goo.gl/IsGzqI
 tar xvzf ipop-14.07.0_ubuntu12.tar.gz
 cd ipop-14.07.0_ubuntu12
```
```bash
  wget -O ipop-14.07.0-x86_64_CentOS6.tar.gz http://goo.gl/3nHK7Z
  tar xvzf ipop-14.07.0-x86_64_CentOS6.tar.gz
  cd ipop-14.07.0-x86_64_CentOS6
```

### Create and configure chat room
A utility script - create_room.py is provided to create and configure the MuC room environment. The room is configured with parameters supplied in the configuration file - room_config.ini. By default the room is members only i.e. A entity can be admitted to the room only if invited. The script should be executed with JID which is to be used to administer the setup and manage users. The config file is as below.

```bash
# user's xmpp credentials for logging into xmpp account
[credentials]
jid : ipopuser@ejabberd
password : password
# IP address/DNS for XMPP server
xmpp_ip : 192.168.14.131 
# File containing configuration parameters for the room.
[parameters]
room_name = ipoptestroom9@conference.ejabberd # name of the room to be created.
room_description : Script configured room
room_persistent : True # room will not self-destruct when the last user leaves the room
room_public : True 
room_password_protected : False # password for room not required
# room password
#room_roomsecret : password
room_maxusers : 400
room_membersonly : True # users admitted only by invitation.
room_members_by_default : True
room_allow_private_messages : True
room_moderated : False
```

To execute the script
```python
python create_room.py -r room_config.ini
```
upon successful execution of the script a room will be created with aforementioned parameters. Since the room is members only, use of room password is redundant and not required. A detailed log file - room_creation.logging is generated , this file captures interaction of the script with XMPP server and can be helpful for troubleshooting if script fails.

### Manage users in the room.
Access to the room and allocation of unique IP4-IPOP addresses to the peers can be done conveniently by making use of the script - manageUsers.py. This script reads a input file - applicants.ini and sends out  room invitations to the JID's(peers) in the file along with handling them IP4 addresses. The script makes use of a file database to keep track of the addresses allocated. It can also be used to block access to users and free addresses allocated to them for reuse.
Sample applicants.ini file.

```bash
# user's xmpp credentials for logging into xmpp account
[credentials]
xmpp_ip_address = 192.168.14.131
jid : ipopuser@ejabberd
password : password
room_name : ipoptestroom9@conference.ejabberd
#room_password : password 
network_prefix : 172.31.0.0 # All peers will get addresses in the range 172.31.XXX.XXX
# JID's of peers on which action is to be taken.
[jids]
#ipoptester@ejabberd : None <--commented out
ipoptester3@ejabberd : None #<--action will only be performed on this JID.
#ipoptester12@ejabberd : None
#ipoptester13@ejabberd : None
#ipoptester14@ejabberd : None
#ipoptester15@ejabberd : None
#ipoptester16@ejabberd : None
#ipoptester17@ejabberd : None
```
To send an invitation to users and allocate IP4 addresses.
```python
python manageUsers.py -i invite -u applicants.ini
```
To block users and free addresses allocated to them.
```python
python manageUsers.py -d delete -u applicants.ini
```  
Blocking a user will not delete the existing IPOP link with it or from it to other peers. Blocked peer will however not be able to establish new links or re-establish trimmed links so to prohibit all connections with it either all other peers will have to restart IPOP or if in ON-DEMAND just wait for the dormant link to be trimmed.

To view current IP/JID allocation.
```python
python manageUsers.py -s show
```
Note:  
1. Use the same JID that was used to create the room to execute this script.  
2. DO NOT delete jid_ip_table.db, ip_table.db and jid_table.db files as they store the mapping between IP's allocated and JIDs.  
3. This script sends out ASYNC invitations to users.  
4 The script only supports network mask 255.255.XXX.XXX, thus only the first two octets can be modified.

### Running adminGVPN  
1. Update the `config.json` file with proper XMPP server address, and the full JID, nickname and password. As mentioned earlier every peer should have a unique JID and nickname. You can use existing public XMPP services,or you can also [[setup your own XMPP server|Installing XMPP Server]].IPv4 addresses will be allocated automatically by MuC room admin by running manageUsers.py script. 

 ```bash
   {
    "xmpp_username": "ipoptester@ejabberd",
    "xmpp_host": "192.168.14.131",
    "nick": "ipoptester",
    "xmpp_password": "password",
    "ip4_mask": 16,
    "controller_logging": "DEBUG",
    "tincan_logging": 2
}

 ```  
2. First, launch the ipop-tincan program

```bash
 sudo sh -c './ipop-tincan-x86_64 1> out.log 2> err.log &'
```  
Note: For 32-bit ubuntu machine use "ipop-tincan-x86" in place of "ipop-tincan-x86_64".  

3. start the admin_gvpn controller  
 ```bash
    chmod 755 admin_gvpn.py
 ```

 ```bash
    ./admin_gvpn.py -c config.json &> log.txt &
 ```
When started the controller will either wait for the invitation from a MuC room or will use cached access details to login to the room. The IP4 address is embedded in the invitation and is cached along with other access details in a file database for subsequent use. In case the user has been blocked by the admin from the room he will not be able to access the XMPP network until invited again. In such a scenario the cached results must be cleared and both the controller and tincan restarted.  

4. Check the network devices and ip address for your device

    ```bash
    /sbin/ifconfig ipop
    ```

    [[ifconfig.png]]  

5. Check status of IPOP links
```bash
awk '/LINK/ || /affiliation/' log.txt
```
  
sample output
```bash
user@ubuntu:~/ipop-14.01.2.rc1-x86_ubuntu12$ awk '/LINK/ || /affiliation/' log.txt
DEBUG:sleekxmpp.xmlstream.xmlstream:RECV: <presence to="ipoptester@ejabberd/1219985521142210607793323" from="ipoptestroom9@conference.ejabberd/ipoptester3" xml:lang="en"><x xmlns="http://jabber.org/protocol/muc#user"><item affiliation="member" role="participant" /></x></presence>
DEBUG:sleekxmpp.xmlstream.xmlstream:RECV: <presence to="ipoptester@ejabberd/1219985521142210607793323" from="ipoptestroom9@conference.ejabberd/ipoptester" xml:lang="en"><x xmlns="http://jabber.org/protocol/muc#user"><item affiliation="member" role="participant" /><status code="110" /></x></presence>
DEBUG:sleekxmpp.plugins.xep_0045:MUC presence from ipoptestroom9@conference.ejabberd/ipoptester3 : {u'lang': 'en', 'status': u'', 'jid': , 'room': u'ipoptestroom9@conference.ejabberd', 'nick': u'ipoptester3', 'show': u'', 'affiliation': 'member', 'role': 'participant', 'alt_nick': <nick xmlns="http://jabber.org/protocol/nick" />}
DEBUG:sleekxmpp.plugins.xep_0045:MUC presence from ipoptestroom9@conference.ejabberd/ipoptester : {u'lang': 'en', 'status': u'', 'jid': , 'room': u'ipoptestroom9@conference.ejabberd', 'nick': u'ipoptester', 'show': u'', 'affiliation': 'member', 'role': 'participant', 'alt_nick': <nick xmlns="http://jabber.org/protocol/nick" />}
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS offline
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS offline
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS offline
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS online
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS online
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS online
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS online
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS online
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS online
DEBUG:gvpn_controller:*********** LINK WITH ipoptestroom9@conference.ejabberd/ipoptester3  IS online

```
if their is no output, it implies the links are not up and their are connectivity issues. In case you were able to connect earlier with the same credentials, it is likely that you have been blocked from the room. To confirm this extract your 'affiliation' status from the log file.

```bash
cat log.txt | grep -i affiliation
```

empty affiliation and role values indicate that you have been blocked. To reconnect remove cache file
```bash
rm access.db
``` 
restart controller, tincan to wait for new invitation and contact the admin to send you a fresh invite.  

Note:  
1. invites are ASYNC and can be lost if consumed in error. If controller has accepted the invite and has access to the room 'affiliation' value must be 'member'. If not delete cache file, restart controller,tincan and request invite again.  
2. ensure you delete the 'access.db' file before restarting and requesting invite as the cached information is stale.

### Stopping admin_gvpn

1.  Kill admin_gvpn

    ```bash
    pkill ipop-tincan-x86_64
    ps aux | grep admin_gvpn.py
    kill -9 <pid-of-admin_gvpn.py>
    ```
    Note: For 32-bit ubuntu machine use "ipop-tincan-x86" in place of "ipop-tincan-x86_64".

***



### On-demand adminGVPN Connection mode

adminGVPN has two modes on establishing the P2P connection. One create P2P connection once it starts to run, the other starts to establish connection when there appears a packet that is destined to a node without P2P connection yet. We call former as proactive mode and latter as on-demand mode. 

### Proactive Connection Mode
 Proactive connection establishes connection to all remote nodes right after it starts to run. The established connections are persistent given that the adminGVPN is running. It is the default mode of operation, but has a drawback of connection overhead when the node number increases. Note that when new node appears on adminGVPN, it establishes connections to all nodes in the adminGVPN network. 

### On-demand Connection Mode
 On-demand connection mode establishes P2P connection only when there is a demand on connection. Technically, when a packet that is destined to a node without P2P connection yet is captured in a tap device, it starts to establish P2P connection. This mode of connection is useful for reducing connection overhead. It also disconnects P2P connection after given threshold of period without traffic.  On-demand connection is configurable through config file. Below two fields are relevant to on-demand connection mode. 

```bash
{
    "on-demand_connection": true,
    "on-demand_inactive_timeout": 600
}
```

Set the "on-demand_connection" field to true allows on-demand connection. Omitting or setting false this field makes the adminGVPN run on default(proactive) mode. "On-demand_inactive_timeout" sets the threshold period of disconnecting P2P. 
