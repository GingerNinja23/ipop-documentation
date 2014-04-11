Tested on Ubuntu 12.04, Debian Wheezy, and CentOS 6.5.

These instructions are derived from these links:

* https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
* https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
* https://sites.google.com/site/webrtc/reference/getting-started

## Download dependencies

1.  Install dependencies

    a. For Ubuntu and Debian

    ```bash
    sudo apt-get install default-jdk pkg-config git subversion make gcc g++ python
    sudo apt-get install libexpat1-dev libgtk2.0-dev libnss3-dev libssl-dev 
    sudo apt-get install libxss-dev libxtst-dev libdbus-1-dev libdrm-dev libgconf2-dev
    sudo apt-get install libgnome-keyring-dev libgcrypt11-dev libpci-dev libudev-dev
    ```

    a. For CentOS 6

    ```bash
    sudo yum install java-1.7.0-openjdk-devel git subversion pkg-config make gcc gcc-c++ python
    sudo yum install expat-devel gtk2-devel libnss3-devel openssl-devel
    ```

2.  Download depot_tools for chromium repo

    ```bash
    mkdir libjingle; cd libjingle
    git clone --depth 1 https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```

3.  Set up environmental variables

    a. For Ubuntu and Debian

    ```bash
    export JAVA_HOME=/usr/lib/jvm/default-java
    export PATH="$(pwd)/depot_tools:$PATH"
    ```

    b. For CentOS 6

    ```bash
    export JAVA_HOME=/usr/lib/jvm/java
    export PATH="$(pwd)/depot_tools:$PATH"
    sudo ln -s /usr/lib64/libpython2.6.so.1.0 /usr/lib/
    ```

4.   (Optional) For 32-bit compilation set target_arch

    ```bash
    export GYP_DEFINES="target_arch=ia32 use_openssl=1"
    ```

## Download source code

1.  Configure gclient to download libjingle code

    ```bash
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.46
    ```

2.  Download libjingle and dependencies (this may take a while). Ignore eror messages 
    about pkg-config looking for `gobject-2.0 gthread-2.0 gtk+-2.0`

    ```bash
    gclient sync --force
    ```

3.  Download ipop-tincan from github.com/ipop-project

    ```bash
    cd trunk/talk; mkdir ipop-project; cd ipop-project
    git clone --depth 1 https://github.com/ipop-project/ipop-tap.git
    git clone --depth 1 https://github.com/ipop-project/ipop-tincan.git
    ```

## Build ipop-tincan for Linux

1.  Return to libjingle trunk directory

    ```bash
    cd ../../
    ```

2.  Copy modified gyp files to trunk/talk directory

    ```bash
    rm -f all.gyp talk/libjingle.gyp talk/ipop-tincan.gyp
    cp talk/ipop-project/ipop-tincan/build/ipop-tincan.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/libjingle.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/all.gyp .
    ```

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

1.  Download socialvpn and groupvpn controllers

    ```
    wget http://github.com/ipop-project/controllers/raw/master/src/svpn_controller.py
    wget http://github.com/ipop-project/controllers/raw/master/src/gvpn_controller.py
    ````
