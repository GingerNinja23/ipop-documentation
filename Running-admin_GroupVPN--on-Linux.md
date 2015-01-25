These instructions are for Ubuntu 12.04 or higher or Debian Wheezy (64-bit). Visit the downloads page to get packages for additional platforms

# Overview of adminGVPN

adminGVPN is a variant of GroupVPN that leverages XMPP XEP0045 (MuC - Multi User Chat Rooms) to bootstrap IPOP links.
Unlike SVPN, GVPN where XMPP functionality is embedded in libjingle, in this setup XMPP interaction is handled in the controller.
adminGVPN obviates the need to manually establish one to one friendship between participating JIDs(users).
In adminGVPN all participating peers must be members of a MuC room, to set up the environment conveniently utility scripts are provided along with the controller to create/configure the room and manage users in the room.

# Prerequisites
1. access to a XMPP service/server with rights to create/configure/manage MuC rooms.
2. each participating entity must have a unique JID and nickname.

# Download and install dependency package.

To abstract XMPP interaction in the controller adminGVPN depends on SleekXMPP, sleekXMPP can be installed easily by following the below steps.
```bash
    sudo apt-get install python-pip
    sudo pip install sleekxmpp
```

# Create and configure chat room
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

# Manage users in the room.
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
To view current IP/JID allocation.
```python
python manageUsers.py -s show
```
Note:
1. Use the same JID that was used to create the room to execute this script./n
2. DO NOT delete jid_ip_table.db, ip_table.db and jid_table.db files as they store the mapping between IP's allocated and JIDs.
3. This script sends out ASYNC invitations to users.
4 The script only supports network mask 255.255.XXX.XXX, thus only the first two octets can be modified.
