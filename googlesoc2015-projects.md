## Overview:

[IPOP](http://ipop-project.org) is an open-source user-centric software virtual network allowing end users to define and create their own virtual private networks (VPNs). IPOP virtual networks provide end-to-end IP tunneling over “TinCan” links setup and managed through a control API to create various software-defined VPN overlays. IPOP builds upon the Jingle WebRTC library to create such links, which are then use as a basis to create a P2P routing overlay for easy-to-deploy VPNs with P2P links.

The following Summer of Code project ideas address user needs in the configuration, management, deployability, and security that have been raised by our users.

## 1) “Command and control” management interface for IPOP VPNs

**Summary**: Users have requested Web-based user interfaces to manage their own private IPOP VPNs. This largely entails the ability to configure and manage XMPP, STUN, TURN servers, and generate configuration files for IPOP endpoints - currently, these tasks are done primarily with command-line interfaces (for XMPP servers) and manual configuration of IPOP endpoints. A Web-based application would greatly lower the barrier of use and adoption of IPOP. 

**Requirements**: The user interface support the following functions:

### VPN configuration:
* Create new VPN groups 
* Add and remove VPN endpoints to a group 
* Configure the IP address space of a VPN group
* Assign unique virtual IP addresses to VPN endpoints 
* Add and remove STUN and TURN endpoints to a VPN group
* Generate IPOP configuration files for VPN endpoints

### Monitoring:
* Collect performance information from IPOP nodes - uptime, number of links, types of links (STUN, TURN), traffic
* Collect error/warning logs from IPOP nodes
* Monitoring of status of STUN/TURN/XMPP servers

### Visualization:
* Mapping of VPN endpoints, STUN, TURN, XMPP servers and links on a map

**Mentor**:  
Saumitra Aditya

**Skills needed**:  
Strong command over Python, basic understanding of computer networks and software design.

**Deliverables**:  
GUI front-end application,encompassing above listed requirements.

## 2) Support for additional virtual device endpoints and tincan links

**Summary**: Users have requested support for OS/X, iOS, and the ability to create links over BlueTooth to cover a wider range of devices and support ad-hoc pairing.

**Requirements**: 
* Extend IPOP’s tap module to support virtual network interfaces available for OS/X and iOS
* Extend IPOP’s tincan module (and jingle if needed) to support ad-hoc links over BlueTooth

**Mentor**: Kyuho Jeong

**Skills needed**: 
* Familiarity with Mac OS X developing environment
* Understanding on Mac KEXT (Kernel Extension) API
* Network programming skill in C/C++ with highly adept at bit/bytewise operation
* Basic understanding on OSI network stack
* Understanding cross-platform development environment (Windows/Linux/OSX)
* Understanding on several prevalent protocols on layer 2,3 and 4 (TCP, UDP, ARP, IP, ICMP, DNS, UPnP)


**Deliverables**: 
* Implement Mac KEXT socket interface to communicate with TinCan
* Implement shim APIs to configure network parameters such as MTU, IPv4/IPv6 Address, MAC address.
* Make frames/packets with destined to certain range of IP address traverse from host to TinCan through KEXT APIs.
* Assign IPv4/IPv6 address on BlueTooth device and runs Ipop to tunnel frames/packets.


## 3) Service authentication improvements

**Summary**  
Users have requested the ability to authenticate IPOP endpoints without storing XMPP username/password on a configuration file. Furthermore, currently tincan does not authenticate requests from a controller running on the local host

**Requirements**  
* Extend IPOP’s tincan module (and jingle if needed) to allow X.509-based authentication to XMPP server.  
* Extend IPOP’s tincan module and controller library to allow authenticated controller/tincan API calls and notifications.  
* Extend IPOP’s controller such that the XMPP credentials are only stored in, and retrieved from a encrypted container.  
* Extend IPOP’s controller such that credentials are requested interactively if they are not stored.  
  
 
**Mentor**  
Ken Subratie

**Skills needed**  
Minimum: Python, socket programming.  
Desirable: C++, IP networks, authentication & encryption, process debugging.

**Deliverables**  
* Encrypted container for storing and managing IPOP credentials.
* User interface to interactively request XPMM credentials.
* Enhancements to IPOP tincan to support a challenge–response authentication to the controller and encrypted communication over its API.
* Enhancements to tincan to support X.509-based authentication to XMPP server.  

## 4) Support for additional online social network back-ends

**Summary**: Users have requested the ability to use various online social networks (e.g. Facebook, Twitter, LinkedIn) to create Social VPNs. Currently, IPOP only supports OSNs that support the XMPP protocol for messaging.

**Requirements**:
* Implement a connectivity layer that extend IPOP/SocialVPN to support OSN-specific APIs at least one additional major OSN (Facebook, Twitter, or LinkedIn) - possibly through a proxy layer that translates XMPP requests used by libjingle and IPOP into OSN-specific actions

**Mentor**: Renato Figueiredo

**Skills needed**: Ability to use OSN REST APIs, XMPP, C++, Python

**Deliverables**: IPOP code extensions supporting at least one major additional OSN.