## Introduction

IPOP allows you to connect to existing XMPP/STUN/TURN services available on the Internet - for instance, Google runs XMPP and STUN services, and there are commercial TURN services. 

It is also possible - and in many environments, desirable - for you to deploy your own private XMPP and NAT traversal services using the ejabberd open-source software and an open-source TURN implementation. This document overviews the process of deploying these services; other documents in the Wiki provide details on how to deploy these services.

## Online Social Network (OSN) and the XMPP service

The XMPP service is used to store information about users and their social network relationships. These are used by IPOP to determine who to establish links with. The ejabberd open-source XMPP server allows you to create a private OSN where you have the flexibility of creating your own users and managing your own relationships. These can be done either through a command-line interface, or a Web interface.

Creating your private XMPP service entails the following tasks - for example, [[here are instructions for Ubuntu|Installing XMPP Server]]

1. Installing ejabberd on a Linux server that is going to be accessible by all IPOP nodes
2. Configuring ejabberd with users and relationships
3. Optionally configuring ejabberd to also serve as a STUN server
