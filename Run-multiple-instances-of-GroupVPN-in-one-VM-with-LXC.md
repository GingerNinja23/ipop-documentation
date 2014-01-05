This tutorial shows how to use gvpn_lxc.sh script, which helps easily desploy lxc instances with ipop-tincan, GroupVPN and SocialVPN controllers.

1. Download the script file from github repo.

  ```bash
  wget https://github.com/ipop-project/ipop-scripts/raw/master/gvpn_lxc.sh
  ``

2. Make it executable. Simply run this script will show help session. 
 ```bash
  sudo chmod +x 
  ./gvpn_lxc.sh
  ```

This script consists of two operation mode. Mode 1 simply create LXC instance and clones it into multiple instances. Mode 2 copies the ipop-tincan and controllers to each instances. I did not merge two steps, since the tincan and controllers are in active development with frequent updates. 

3. Below command should make three instances of LXC with name starts with ipop. 
   ```bash
   ./gvpn_lxc -m 1 -p ipop -i 3 
   ./sudo lxc-ls --fancy (or sudo lxc-list)
   ```

4. Check your lxc version with below command. If it is below 1.0, it does not support all features of LXC such as lxc-attach. Thus, you should comment the block of code that contains "lxc-atach" and uncomment the above block of code, which is using ssh to run script in remote host. 

   ```bash
   dpkg -l | grep lxc
   ```

5. Place your ipop-tincan binary and controllers at your working directory and run below. In this example, the script copies the executables and configure its ipop node with IP address 172.16.0.2, 172.16.0.3 and 172.16.0.4 consecutively with the instance name ipop0, ipop1 and ipop2 respectively. 

   ```bash
   ./gvpn_lxc -m 2 -p ipop -i 3 -a 172.16.0.2
   ./sudo lxc-ls --fancy (or sudo lxc-list)
   ```
5. You can also see the ipop-tincan and controllers running through ps aux. 

   ```bash
   ps aux | grep ipop-tincan
   ps aux | grep gvpn_controller
   ```

6. You can either logon to each console of LXC instance to run test. 

   ```bash
   sudo lxc-console -n ipop0
   ```

7. Or you can copy the test script and run it through lxc-attach or ssh. The file system of each instances is located under /var/lib/lxc/[instance name]/rootfs/. If you are not using the lxc-attach each instances private address starts from 10.0.3.2.

   ```bash
   cp test.sh /var/lib/lxc/ipop0/rootfs/home/ubuntu/
   sudo lxc-attach -n ipop0 /home/ubuntu/test.sh ( for lxc 1.0 or higher)
   ssh -n 10.0.3.2 /home/ubuntu/test.sh
   ```
  
