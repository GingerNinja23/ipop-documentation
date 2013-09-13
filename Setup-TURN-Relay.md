Sometimes SocialVPN nodes cannot connect because of crazy NATs (i.e. symmetric) or strict firewalls. In such cases, we rely on relaying. This page shows you how to run a TURN relay in the cloud. We are currently using the following TURN implementation. TURN servers require a public IP address. These instructions have only been tested on Ubuntu 12.04 (64-bit).

http://turnserver.sourceforge.net/

1. Install TURN implementation dependencies
```bash
sudo apt-get update
sudo apt-get install libconfuse0
```
2. If you are running on the cloud (e.g. EC2), you need to use IP aliasing to allow the TURN server to bind your public IP address
```bash
sudo ifconfig eth0:0 <public-ip-of-vm> up
```
3. Download and extract pre-packaged TURN binaries
```bash
wget -O turn.tgz http://goo.gl/EMzy9Z
tar xzvf turn.tgz
```
4. Update the TURN config file with the public IP address
```bash
sed -i 's/listen_address = .*/listen_address = { "public-ip-address" }/g' turn/turn.conf
```
5. Run the TURN server
```bash
cd turn; ./turnserver -c turn.conf; cd ..
```
6. Verify TURN server is running
```bash
netstat -aupn | grep 19302
```
7. Update your svpn controller with the TURN settings https://github.com/socialvpn/svpn-jingle/blob/master/src/controllers/svpn_controller.py
```python
TURN = "public-ip-address-turn-server:19302"
TURN_USER = "svpnjingle"
TURN_PASS = "1234567890"
```
8. If you are using the lxc script you can add turn settings to script https://github.com/socialvpn/svpn-jingle/blob/master/scripts/svpn_lxc.sh#L31
```bash
TURN="public-ip-address-turn-server:19302"
TURN_USER="svpnjingle"
TURN_PASS="1234567890"
```
```