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