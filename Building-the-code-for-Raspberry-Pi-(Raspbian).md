These instructions have been tested on Raspbian on a Raspberry Pi device.

These instructions are derived from these links:

* https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
* https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
* https://sites.google.com/site/webrtc/reference/getting-started

### Install necessary libraries and chromium tools

1.  This works on Debian-based distros

    ```bash
    sudo apt-get install libexpat1-dev git subversion build-essential binutils-gold
    ```

2.  Download depot_tools for chromium repo

    ```bash
    git clone --depth 1 -b master https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```

3.  Set up environmental variables

    ```bash
    export JAVA_HOME=/usr/lib/jvm/jdk-7-oracle-armhf/
    export PATH="$(pwd)/depot_tools:$PATH"
    export GYP_GENERATORS="make"
    export GYP_DEFINES="target_arch=arm arm_version=6"
    export C_INCLUDE_PATH=/usr/include:/usr/include/arm-linux-gnueabihf
    export CPLUS_INCLUDE_PATH=/usr/include:/usr/include/arm-linux-gnueabihf
    ```

### Get the libjingle and ipop-tincan source code

1.  Configure gclient to download libjingle code

    ```bash
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.46
    ```

2.  Download libjingle and dependencies (this takes a while)

    ```bash
    gclient sync --force
    ```

3.  Download ipop-tincan from github.com/ipop-project

    ```bash
    cd trunk/talk; mkdir ipop-project; cd ipop-project
    git clone --depth 1 -b master https://github.com/ipop-project/ipop-tap.git
    git clone --depth 1 -b master https://github.com/ipop-project/ipop-tincan.git
    ```

### Building ipop-tincan

1.  Return to libjingle trunk directory

    ```bash
    cd ../../
    ```

2.  Copy modified gyp files to trunk/talk directory

    ```bash
    cp talk/ipop-project/ipop-tincan/build/ipop-tincan.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/libjingle.gyp talk/
    cp talk/ipop-project/ipop-tincan/build/all.gyp .
    ```

3.  Update build/common.gypi by setting "arm_fpu" to "vfp" and "arm_float_abi" to "hard" for arm_version 6

    ```bash
    sed -i "s/'arm_float_abi%': 'soft',/'arm_float_abi%': 'hard',/g" build/common.gypi 
    sed -i "s/'arm_fpu%': '',/'arm_fpu%': 'vfp',/g" build/common.gypi
    ```

4.  Update the gold linker to arm version

    ```bash
    mv third-party/gold/gold32 third-party/gold/gold32.bak
    ln -s /usr/bin/gold third-party/gold/gold32
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
