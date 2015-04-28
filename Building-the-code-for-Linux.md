Tested on Ubuntu 12.04, Debian 7, and CentOS 6.5.

These instructions are derived from these links:

* https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
* https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
* https://sites.google.com/site/webrtc/reference/getting-started

## Download dependencies

1.  Install dependencies

    a. For Ubuntu and Debian

    ```bash
    sudo apt-get update
    sudo apt-get install default-jdk pkg-config git subversion make gcc g++ python
    sudo apt-get install libexpat1-dev libgtk2.0-dev libnss3-dev libssl-dev 
    ```

    b. For CentOS 6.5

    ```bash
    sudo yum install java-1.7.0-openjdk-devel git subversion pkg-config make gcc gcc-c++ python
    sudo yum install expat-devel gtk2-devel nss-devel openssl-devel
    sudo wget http://people.centos.org/tru/devtools-1.1/devtools-1.1.repo -P /etc/yum.repos.d
    sudo sh -c 'echo "enabled=1" >> /etc/yum.repos.d/devtools-1.1.repo'
    sudo yum install devtoolset-1.1
    ```

2.  Download depot_tools for chromium repo

    ```bash
    mkdir libjingle; cd libjingle
    git clone --depth 1 https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```

3.  Set up environmental variables and prepare local environment

    a. For Ubuntu and Debian

    ```bash
    export JAVA_HOME=/usr/lib/jvm/default-java
    export PATH="$(pwd)/depot_tools:$PATH"
    export GYP_DEFINES="use_openssl=1"
    ```

    b. For CentOS 6.5

    ```bash
    export JAVA_HOME=/usr/lib/jvm/java
    export PATH="$(pwd)/depot_tools:$PATH"
    export GYP_DEFINES="use_openssl=1"
    sudo ln -s /usr/lib64/libpython2.6.so.1.0 /usr/lib/
    scl enable devtoolset-1.1 bash
    ```

    ```bash
    curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
    export PATH="$HOME/.pyenv/bin:$PATH"
    eval "$(pyenv init -)"
    eval "$(pyenv virtualenv-init -)"
    yum install zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel mysql-devel
    pyenv install 2.7.9
    pyenv local 2.7.9
    ```

4.  (Optional) For 32-bit compilation set target_arch

    ```bash
    export GYP_DEFINES="$GYP_DEFINES target_arch=ia32"
    ```

## Download source code

1.  Configure gclient to download libjingle code

    ```bash
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.52
    ```

2.  Download libjingle and dependencies (this may take a while). Ignore eror messages 
    about pkg-config looking for `gobject-2.0 gthread-2.0 gtk+-2.0`

    ```bash
    gclient sync --force
    ```

    _On CentOS, you will see some errors, ignore them_

3.  Download ipop-tincan from github.com/ipop-project

    ```bash
    cd trunk/talk; mkdir ipop-project; cd ipop-project
    git clone --depth 1 https://github.com/ipop-project/ipop-tap.git
    git clone --depth 1 https://github.com/ipop-project/ipop-tincan.git
    ```
4.  Return to libjingle trunk directory

    ```bash
    cd ../../
    ```

5.  Copy modified gyp files to trunk/talk directory

    ```bash
    rm -f DEPS all.gyp talk/libjingle.gyp talk/ipop-tincan.gyp
    cp talk/ipop-project/ipop-tincan/build/ipop-tincan.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/libjingle.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/all.gyp .
    cp talk/ipop-project/ipop-tincan/build/DEPS .
    ```

1.  Sync again to download OpenSSL from chromium repository

    ```bash
    gclient sync --force
    ```

## Build ipop-tincan for Linux

3.  Generate ninja build files

    ```bash
    gclient runhooks --force
    ```

4.  Build tincan for linux (binary localed at out/Release/ipop-tincan)

    ```bash
    ninja -C out/Release ipop-tincan
    ```

5.  To build debug version with gdb symbols (but creates 25 MB binary)

    ```bash
    ninja -C out/Debug ipop-tincan
    ```

6.  The generated binary is located at `out/Release/ipop-tincan` or
    `out/Debug/ipop-tincan`

## Download controllers

1.  Download SocialVPN and GroupVPN controllers

    ```
    wget http://github.com/ipop-project/controllers/raw/devel/src/ipoplib.py
    wget http://github.com/ipop-project/controllers/raw/devel/src/svpn_controller.py
    wget http://github.com/ipop-project/controllers/raw/devel/src/gvpn_controller.py
    ````