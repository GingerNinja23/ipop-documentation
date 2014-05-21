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

5.  Update configuration

    ```bash
    sudo vi /etc/ejabberd/ejabberd.cfg

    #update lines with ipopuser and ejabberd host
    %% Admin user
    {acl, admin, {user, "ipopuser", "ejabberd"}}.

    %% Hostname
    {hosts, ["localhost", "ejabberd"]}.
    ```

6.  Add the following to the ejabberd.cfg file to enable STUN
    ```bash
    sudo vi /etc/ejabberd/ejabberd.cfg

    {listen,
     [
      {5222, ejabberd_c2s, [
                            {access, c2s},
                            {shaper, c2s_shaper},
                            {max_stanza_size, 65536},
                            %%zlib,
                            starttls, {certfile, "/etc/ejabberd/ejabberd.pem"}
                           ]},

      %% this is the new line to enable stun
      {{3478, udp}, ejabberd_stun, []},
    ```

5.  (Optional) Create a new self-signed certificate. If you change the hostname from _ejabberd_
    to something else, you need to create a new self-signed certificate and set CN (common name) to
    the new hostname that you have chosen and place it under /ect/ejabberd

    ```bash
    openssl req -x509 -nodes -days 3650 -newkey rsa:1024 -keyout ejabberd.pem -out ejabberd.pem
    ```

6.  Restart ejabberd service

    ```bash
    /etc/init.d/ejabberd start
    ```

7.  Create default user

    ```bash
    sudo ejabberdctl register ipopuser ejabberd password
    ```

8. Enable it at boot time

    ```
    chkconfig ejabberd on
    ```
