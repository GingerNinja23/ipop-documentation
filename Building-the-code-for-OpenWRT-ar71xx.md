These are the build instructions for the [OpenWRT D-LINK DIR-505](http://wiki.openwrt.org/toh/d-link/dir-505) that we have purchased from [Amazon](http://www.amazon.com/D-Link-Systems-SharePort-Companion-DIR-505L/dp/B009LENJ90) for testing. [Click here for instructions on how to flash your router](http://pstjuste.blogspot.com/2013/11/installing-openwrt-on-d-link-dir-505l.html).

Go on the [[Building the code for Linux page|Building the code for Linux]]
and follow the instructions for the first two sections:

* Download dependencies
* Download source code

## Download OpenWRT toolchain

1. Install dependencies

   ```bash
   sudo dpkg add-architecture i386; sudo apt-get update
   sudo apt-get install ccache libncurses5-dev zlib1g-dev gawk unzip libc6:i386 libstdc++6:i386 zlib1g:i386
   ```

2.  Go to trunk/third-party directory and download OpenWRT SDK for openwrt ar71xx

    ```bash
    cd ../../third-party
    wget http://downloads.openwrt.org/attitude_adjustment/12.09/ar71xx/generic/OpenWrt-SDK-ar71xx-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2.tar.bz2
    tar xjvf OpenWrt-SDK-ar71xx-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2.tar.bz2
    ```

3.  Set up OpenWRT environmental variables

    ```bash
    export OPENWRT_SDK=`pwd`/OpenWrt-SDK-ar71xx-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2
    export STAGING_DIR=$OPENWRT_SDK/staging_dir
    export TOOLCHAIN=$STAGING_DIR/toolchain-mips_r2_gcc-4.6-linaro_uClibc-0.9.33.2/
    export CC="$TOOLCHAIN/bin/mips-openwrt-linux-uclibc-gcc"
    export CXX="$TOOLCHAIN/bin/mips-openwrt-linux-uclibc-g++"
    export AR="$TOOLCHAIN/bin/mips-openwrt-linux-uclibc-ar"
    export CC_host="gcc"
    export CXX_host="g++"
    export GYP_DEFINES="target_arch=mips use_openssl=1"
    ```

4. Install libexpat for OpenWRT toolchain

    ```bash
    cd $OPENWRT_SDK
    svn export svn://svn.openwrt.org/openwrt/branches/packages_12.09/libs/expat package/expat; make
    cp build_dir/target-mips_r2_uClibc-0.9.33.2/expat-2.0.1/.libs/libexpat.a $TOOLCHAIN/lib
    cp build_dir/target-mips_r2_uClibc-0.9.33.2/expat-2.0.1/lib/*.h $TOOLCHAIN/include/
    cp -r /usr/include/X11 $TOOLCHAIN/include/
    cd ../../
    ```

## Build ipop-tincan for OpenWRT

1.  Create ninja build files

    ```bash
    gclient runhooks --force
    ```

2. Update ninja build files to use ```-msoft-float``` and ```-fno-stack-protector```

    ```bash
    sed -i 's/mhard-float/msoft-float/g' `find out/Release -name *.ninja`
    sed -i 's/fstack-protector/fno-stack-protector/g' `find out/Release -name *.ninja`
    sed -i 's/mhard-float/msoft-float/g' `find out/Debug -name *.ninja`
    sed -i 's/fstack-protector/fno-stack-protector/g' `find out/Debug -name *.ninja`
    ```

2. Update some files file

    ```bash
    wget https://github.com/pstjuste/ipop-tincan/raw/75e2c5fae7b6375ef2ead4a93595275492a6a259/build/typedefs.h
    wget https://github.com/pstjuste/ipop-tincan/raw/75e2c5fae7b6375ef2ead4a93595275492a6a259/build/ipop-tincan.ninja
    move typedefs.h webrtc/typedefs.h
    move ipop-tincan.ninja out/Release/obj/talk/ipop-tincan.ninja
    cd ..
    
    ```
3.  Build tincan for OpenWrt (binary located at out/Release/ipop-tincan)

    ```bash
    ninja -C out/Release ipop-tincan
    ```

4.  To build debug version with gdb symbols (but creates 25 MB binary)

    ```bash
    ninja -C out/Debug ipop-tincan
    ```
5.  The generated binary is located at `out/Release/ipop-tincan` or
    `out/Debug/ipop-tincan`

## Download controllers

1.  Download socialvpn and groupvpn controllers

    ```
    wget http://github.com/ipop-project/socialvpn/raw/master/src/svpn_controller.py
    wget http://github.com/ipop-project/groupvpn/raw/master/src/gvpn_controller.py
    ````