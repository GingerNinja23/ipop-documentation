How to install ejabberd on a RHEL 6.5 running on the RHN.

1. RHEL Requires that your computer be subscribed to the appropriate channels.  Thus, you must add the "epel" and "optional" channels. To do this, Type the following commands (you will need a RHN username/password combination that allows you to add channels):

    ```
    rhn-channel -a -c epel_rhel6_x86_64
    rhn-channel -a -c rhel-x86_64-server-optional-6
    ```

1. Before you can install EPEL packages, you must have a GPG certificate.  To get it, use this command:

    ```
    yum install epel-release --nogpgcheck
    ```

3. **This step is for CentOS 6.5 Only, not RHEL 6.5**

    ```bash
    # for 64-bit
    rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    # for 32-bit
    rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
    ```

4. Install ejabberd

    ```
    yum install ejabberd
    ```

5. Enable it at boot time

    ```
    chkconfig ejabberd on
    ```

6. Get/install the config file

    ```
    cd /etc/ejabberd
    mv ejabberd.cfg ejabberd.cfg.orig
    wget -O ejabberd.cfg http://goo.gl/iObOjl --no-check-certificate
    chown ejabberd.ejabberd ejabberd.cfg
    ```

7. Start it

    ```
    /etc/init.d/ejabberd start
    ```

8. Add the default user

    ```
    ejabberdctl register svpnuser ejabberd password
    ```
