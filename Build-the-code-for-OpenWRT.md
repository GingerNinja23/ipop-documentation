This is for OpenWRT malta device which is designed for Qemu Mipsel (http://wiki.openwrt.org/doc/howto/qemu).

Go on the Building the code for Linux page (https://github.com/ipop-project/ipop-tincan/wiki/Building-the-code-for-Linux) and follow the instructions for the following sections

* Install Java
* Install necessary libraries and chromium tools
* Get the libjingle and ipop-tincan source code

## Set up OpenWRT build environment and build code

1.  Go to trunk/third-party directory and download OpenWRT SDK for openwrt malta

    ```bash
    cd ../../third-party
    wget http://downloads.openwrt.org/attitude_adjustment/12.09-rc1/malta/generic/OpenWrt-SDK-malta-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2.tar.bz2
    tar xjvf OpenWrt-SDK-malta-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2.tar.bz2
    ln -s OpenWrt-SDK-malta-for-linux-i486-gcc-4.6-linaro_uClibc-0.9.33.2 openwrt-sdk
    ```
2. Install libexpat in openwrt-sdk

    ```bash
    cd openwrt-sdk
    svn export svn://svn.openwrt.org/openwrt/packages/libs/expat package/expat
    sudo apt-get install ccache
    make
    cp build_dir/target-mipsel_r2_uClibc-0.9.33.2/expat-2.0.1/.libs/libexpat.a staging_dir/toolchain-mipsel_r2_gcc-4.6-linaro_uClibc-0.9.33.2/lib
    cp build_dir/target-mipsel_r2_uClibc-0.9.33.2/expat-2.0.1/lib/*.h staging_dir/toolchain-mipsel_r2_gcc-4.6-linaro_uClibc-0.9.33.2/include/
    cd ../../
    ```

2.  Set up OpenWRT environmental variables

    ```bash
    export GYP_DEFINES="target_arch=mipsel"
    export STAGING_DIR="`pwd`/third-party/openwrt-sdk/staging_dir"
    export CC="$STAGING_DIR/toolchain-mipsel_r2_gcc-4.6-linaro_uClibc-0.9.33.2/bin/mipsel-openwrt-linux-uclibc-gcc"
    export CXX="$STAGING_DIR/toolchain-mipsel_r2_gcc-4.6-linaro_uClibc-0.9.33.2/bin/mipsel-openwrt-linux-uclibc-g++"
    export AR="$STAGING_DIR/toolchain-mipsel_r2_gcc-4.6-linaro_uClibc-0.9.33.2/bin/mipsel-openwrt-linux-uclibc-ar"
    export CC_host="gcc"
    export CXX_host="g++"
    ```

3.  Create ninja build files

    ```bash
    gclient runhooks --force
    ```

4.  Build tincan for OpenWrt (binary located at out/Release/ipop-tincan)

    ```bash
    ninja -C out/Release ipop-tincan
    ```

5.  To build debug version with gdb symbols (but creates 25 MB binary)

    ```bash
    ninja -C out/Debug ipop-tincan
    ```