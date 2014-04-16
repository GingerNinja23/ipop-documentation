These instructions have been tested on Raspbian on a Raspberry Pi device.

These instructions are derived from these links:

* https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
* https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
* https://sites.google.com/site/webrtc/reference/getting-started

## Download dependencies

1.  This works on Debian-based distros

    ```bash
    sudo apt-get update
    sudo apt-get install pkg-config git subversion make gcc g++ python binutils-gold
    sudo apt-get install libexpat1-dev libgtk2.0-dev libnss3-dev libssl-dev
    ```

2.  Download depot_tools for chromium repo

    ```bash
    mkdir libjingle; cd libjingle
    git clone --depth 1 https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```

3.  Set up environmental variables

    ```bash
    export JAVA_HOME=/usr/lib/jvm/jdk-7-oracle-armhf/
    export PATH="$(pwd)/depot_tools:$PATH"
    export GYP_GENERATORS="make"
    export GYP_DEFINES="target_arch=arm arm_version=6 use_openssl=1"
    export C_INCLUDE_PATH=/usr/include:/usr/include/arm-linux-gnueabihf
    export CPLUS_INCLUDE_PATH=/usr/include:/usr/include/arm-linux-gnueabihf
    ```

## Download source code

1.  Configure gclient to download libjingle code

    ```bash
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.52
    ```

2.  Download libjingle and dependencies (this takes a while)

    ```bash
    gclient sync --force
    ```

3.  Download ipop-tincan from github.com/ipop-project

    ```bash
    cd trunk/talk; mkdir ipop-project; cd ipop-project
    git clone --depth 1 https://github.com/ipop-project/ipop-tap.git
    git clone --depth 1 https://github.com/ptony82/ipop-tincan.git -b feature/migration_3.52
    ```

## Build ipop-tincan for Raspbian

1.  Return to libjingle trunk directory

    ```bash
    cd ../../
    ```

2.  Copy modified gyp files to trunk/talk directory

    ```bash
    rm -f DEPS all.gyp talk/libjingle.gyp talk/ipop-tincan.gyp
    cp talk/ipop-project/ipop-tincan/build/ipop-tincan.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/libjingle.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/all.gyp .
    cp talk/ipop-project/ipop-tincan/build/DEPS .
    ```

3.  Sync again to download OpenSSL from chromium repository

    ```bash
    gclient sync --force
    ```
3.  Update build/common.gypi configurations

    ```bash
    sed -i "s/'arm_float_abi%': 'soft',/'arm_float_abi%': 'hard',/g" build/common.gypi 
    sed -i "s/'arm_fpu%': '',/'arm_fpu%': 'vfp',/g" build/common.gypi
    ```

4.  Update the gold linker to arm version

    ```bash
    mv third_party/gold/gold32 third_party/gold/gold32.bak
    ln -s /usr/bin/gold third_party/gold/gold32
    ```
    
5.  Generate ninja build files

    ```bash
    gclient runhooks --force
    ```

6.  Build tincan for Raspbian (binary localed at out/Release/ipop-tincan)

    ```bash
    make ipop-tincan BUILDTYPE=Release
    ```

7.  To build debug version with gdb symbols (but creates 25 MB binary)

    ```bash
    make ipop-tincan BUILDTYPE=Debug
    ```

8.  The generated binary is located at `out/Release/ipop-tincan` or
    `out/Debug/ipop-tincan`

## Download controllers

1.  Download socialvpn and groupvpn controllers

    ```
    wget http://github.com/ipop-project/controllers/raw/devel/src/ipoplib.py
    wget http://github.com/ipop-project/controllers/raw/devel/src/svpn_controller.py
    wget http://github.com/ipop-project/controllers/raw/devel/src/gvpn_controller.py
    ````
