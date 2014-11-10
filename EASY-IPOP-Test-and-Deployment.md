
ipop-test
=========

For easier deployment and test of ipop, we provide ipop-test script. 
https://github.com/ipop-project/ipop-test

ipop-test provides test harness for ipop and helps deploying multiple ipop-nodes in a single VM. It leverages python unittest framework (https://docs.python.org/2/library/unittest.html). With this script, you can deploy and run multiple ipop-nodes in a single blank VM.

You can run this script in Amazon EC2 or openstack instance then follow below instructions. Or you can also run it with under your VMware with ubuntu 14.04 image(http://www.trendsigma.net/vmware/ubuntu1404t.html).

This script is tested under Ubuntu 14.04.

TEST CLASSES
------------

At this point, there are three test classes. TestInstall and TestLxcCreate classes are the prerequisite classes, so you need to run these two classes before other test class. 

- **TestInstall**

  Install XMPP and LXC in VM.
  
- **TestLxcCreate**

  Create LXC instances. The number of instances can be specified in script(instance_count). 
  
- **TestSocialVPN**

  TestInstall and TestLxcCreate should had run once before this test. This test send ping packet from one node to all the other nodes. For example, if instance_count is set 5, 5 lxc instances created and each instance sends ping to all the other ipop-nodes. In total, 20 different ping command runs. 
  
...

We plan to add more complex and thorough test scenarios alongside with script implementation.


Usage instruction
-----------------

1. Prepare a blank VM(Ubuntu is preferred)

2. download test.py script 

  ```
  wget https://github.com/ipop-project/ipop-test/raw/master/test.py
  chmod +x test.py
  ```

3. Install XMPP and LXC in VM
  ```
  python -m unittest -v test.TestInstall
  ```

  When XMPP/LXC is not installed in VM before, this command takes more than 10 minutes. But, if it is, it only takes several minutes.
  
  It should complete with this output.
  
  ```
  ----------------------------------------------------------------------
  Ran 2 tests in 1094.594s

  OK
  ```

4. Create LXC isntances 

  The number of LXC can be specified in the script. 
  instance_count : number of LXC instances
  ipop_download_link : URL link for ipop-download
  ipop_dir : directory name of IPOP after extraction

  ```
  vi test.py

  instance_count = 3
  ipop_download_link = "https://github.com/ipop-project/downloads/releases/"+\
                     "download/14.07.1.rc2/ipop-14.07.1.rc2-x86_ubuntu.tar.gz"
  ipop_dir = "ipop-14.07.1.rc2-x86_ubuntu"
  ```

  ```
  python -m unittest -v test.TestLxcCreate
  ```

5. Choose the test class you want and run it. For example, TestSocialVPN test class runs SociapVPN ipop in each LXC instances and sends ping from one node to all the other node one by one. This should completes with "OK". 

  ```
  python -m unittest -v test.TestSocialVPN
  ```

6. When you want to stop ipop, you can list lxc instances then stop each instance as below. 

  ```
  sudo lxc-ls --fancy
  sudo lxc-stop -n ipop0
  sudo lxc-stop -n ipop1
  ...
  ```
