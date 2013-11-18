This is for OpenWRT malta device which is designed for Qemu Mipsel (http://wiki.openwrt.org/doc/howto/qemu).

Go on the Building the code for Linux page (https://github.com/ipop-project/ipop-tincan/wiki/Building-the-code-for-Linux) and follow the instructions for the following sections

* Install Java
* Install necessary libraries and chromium tools
* Get the libjingle and ipop-tincan source code

## Set up OpenWRT build environment and build code

1. Install ccache and svn

   ```bash
   sudo apt-get install ccache svn
   ```

2.  Go to trunk/third-party directory and download OpenWRT SDK for openwrt malta

    ```bash
    cd ../../third-party
    wget http://downloads.openwrt.org/attitude_adjustment/12.09-rc1/malta/generic/OpenWrt-SDK-malta-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2.tar.bz2
    tar xjvf OpenWrt-SDK-malta-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2.tar.bz2
    ```

3.  Set up OpenWRT environmental variables

    ```bash
    export OPENWRT_SDK=`pwd`/OpenWrt-SDK-malta-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2
    export STAGING_DIR=$OPENWRT_SDK/staging_dir
    export TOOLCHAIN=$STAGING_DIR/toolchain-mipsel_r2_gcc-4.6-linaro_uClibc-0.9.33.2/
    export CC="$TOOLCHAIN/bin/mipsel-openwrt-linux-uclibc-gcc"
    export CXX="$TOOLCHAIN/bin/mipsel-openwrt-linux-uclibc-g++"
    export AR="$TOOLCHAIN/bin/mipsel-openwrt-linux-uclibc-ar"
    export CC_host="gcc"
    export CXX_host="g++"
    export GYP_DEFINES="target_arch=mipsel"
    ```

4. Install libexpat for OpenWRT toolchain

    ```bash
    cd $OPENWRT_SDK
    svn export svn://svn.openwrt.org/openwrt/packages/libs/expat package/expat; make
    cp build_dir/target-mipsel_r2_uClibc-0.9.33.2/expat-2.0.1/.libs/libexpat.a $TOOLCHAIN/lib
    cp build_dir/target-mipsel_r2_uClibc-0.9.33.2/expat-2.0.1/lib/*.h $TOOLCHAIN/include/
    cd ../../
    ```

5.  Create ninja build files

    ```bash
    gclient runhooks --force
    ```

6. Update ninja build files to use ```-msoft-float``` and ```-fno-stack-protector```

    ```bash
    sed -i 's/mhard-float/msoft-float/g' `find out/Release -name *.ninja`
    sed -i 's/fstack-protector/fno-stack-protector/g' `find out/Release -name *.ninja`
    sed -i 's/mhard-float/msoft-float/g' `find out/Debug -name *.ninja`
    sed -i 's/fstack-protector/fno-stack-protector/g' `find out/Debug -name *.ninja`
    ```

6.  Build tincan for OpenWrt (binary located at out/Release/ipop-tincan)

    ```bash
    ninja -C out/Release ipop-tincan
    ```

7.  To build debug version with gdb symbols (but creates 25 MB binary)

    ```bash
    ninja -C out/Debug ipop-tincan
    ```