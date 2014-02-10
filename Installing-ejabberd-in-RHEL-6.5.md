How to install ejabberd on a RHEL 6.5 running on the RHN.

RHEL Requires that your computer be subscribed to the appropriate channels.  Thus, you must add the "epel" and "optional" channels. To do this, Type the following commands (you will need a RHN username/password combination that allows you to add channels):

    rhn-channel -a -c epel_rhel6_x86_64
    rhn-channel -a -c rhel-x86_64-server-optional-6

Before you can install EPEL packages, you must have a GPG certificate.  To get it, use this command:

    yum install epel-release --nogpgcheck

Install ejabberd

    # for 64-bit
    rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

    # for 32-bit
    rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm

    yum install ejabberd

Enable it at boot time

    chkconfig ejabberd on

Get/install the config file

    cd /etc/ejabberd
    mv ejabberd.cfg ejabberd.cfg.orig
    wget -O ejabberd.cfg http://goo.gl/iObOjl --no-check-certificate
    chown ejabberd.ejabberd ejabberd.cfg

Start it

    /etc/init.d/ejabberd start

Add the default user

    ejabberdctl register svpnuser ejabberd password
