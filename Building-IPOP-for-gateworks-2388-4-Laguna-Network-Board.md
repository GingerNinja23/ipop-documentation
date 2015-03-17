## Download dependencies

1.  Install dependencies

    a. For Ubuntu and Debian

    ```bash
    sudo apt-get update
    sudo apt-get install default-jdk pkg-config git subversion make gcc g++ python
    sudo apt-get install libexpat1-dev libgtk2.0-dev libnss3-dev libssl-dev 
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
    export GYP_DEFINES="use_openssl=1"
    export GYP_DEFINES="$GYP_DEFINES target_arch=arm arm_version=6"
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
This will take a few minutes 5~10 depending upon download speed, you will encounter the following errors, ignore them for the moment--
```bash
________ running '/usr/bin/python trunk/webrtc/build/gyp_webrtc -Dextra_gyp_flag=0' in '/home/saumitra/laguna_ipop'
Updating projects from gyp files...
/bin/sh: 1: ../../../build/linux/pkg-config-wrapper: not found
gyp: Call to '../../../build/linux/pkg-config-wrapper "/home/saumitra/laguna_ipop/trunk/arm-sysroot" "arm" --libs-only-L --libs-only-other nss' returned exit status 127.
gyp: /home/saumitra/laguna_ipop/trunk/third_party/openssl/openssl.gyp not found (cwd: /home/saumitra/laguna_ipop)
Error: Command /usr/bin/python trunk/webrtc/build/gyp_webrtc -Dextra_gyp_flag=0 returned non-zero exit status 1 in /home/saumitra/laguna_ipop

```  
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
To deal with them, follow the below steps  
6. Find out files, where pkg-config-wrapper is defined  
```bash
saumitra@ipop1-ThinkPad-T520:trunk$ grep -Ir --exclude=\*.{c,h} "../../../build/linux/pkg-config-wrapper" *
net/third_party/nss/ssl.gyp~:            'pkg-config': '../../../build/linux/pkg-config-wrapper "<(sysroot)" "<(target_arch)"',
net/third_party/nss/.svn/pristine/42/4212e92ed61dc2a0bfaa0a23ae0703dcd811161c.svn-base~:            'pkg-config': '../../../build/linux/pkg-config-wrapper "<(sysroot)" "<(target_arch)"',
talk/ipop-project/ipop-tincan/build/libjingle.gyp:           'pkg-config': '../../../build/linux/pkg-config-wrapper "<(sysroot)" "<(target_arch)"',
talk/libjingle.gyp~:           'pkg-config': '../../../build/linux/pkg-config-wrapper "<(sysroot)" "<(target_arch)"',
talk/libjingle.gyp:           'pkg-config': '../../../build/linux/pkg-config-wrapper "<(sysroot)" "<(target_arch)"',
```   
7. For all the .gyp files,edit the value of 'pkg-config' variable at the top to absolute path of pkg-config-wrapper,which will be in trunk/linux/build.  
```python
# Copyright (c) 2012 The Chromium Authors. All rights reserved.
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.

{
  'conditions': [
    [ 'os_posix == 1 and OS != "mac" and OS != "ios"', {
      'conditions': [
        ['sysroot!=""', {
          'variables': {
            'pkg-config': '/home/saumitra/laguna_ipop/trunk/build/linux/pkg-config-wrapper "<(sysroot)" "<(target_arch)"',
          },
        }, {
          'variables': {
            'pkg-config': 'pkg-config'
          },
        }],
      ],
    }],
  ],  
```    
8. Just to be sure set few environment variables again  
```bash
export GYP_DEFINES="use_openssl=1"
export GYP_DEFINES="$GYP_DEFINES target_arch=arm arm_version=6" 
```  
 
execute 'gclient sync --force in trunk directory again to download openssl and this time there will be no errors. 

9. Use the below command  in trunk directory to set hard-fp abi   
```bash
sed -i "s/'arm_float_abi%': 'soft',/'arm_float_abi%': 'hard',/g" build/common.gypi
sed -i "s/'arm_fpu%': '',/'arm_fpu%': 'vfp',/g" build/common.gypi
```    
## Set up Gatework toolchain  

10. Get gateworks laguna SDK and set corresponding environment variables and few dependencies
```bash
cd third_party/
wget http://dev.gateworks.com/openwrt/latest/cns3xxx/OpenWrt-SDK-cns3xxx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2
tar xvfj OpenWrt-SDK-cns3xxx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2 
export OPENWRT_SDK=`pwd`/OpenWrt-SDK-cns3xxx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2
export STAGING_DIR=$OPENWRT_SDK/staging_dir
export TOOLCHAIN=$STAGING_DIR/toolchain-arm_mpcore+vfp_gcc-4.8-linaro_uClibc-0.9.33.2_eabi
export CC="$TOOLCHAIN/bin/arm-openwrt-linux-uclibcgnueabi-gcc"
export CXX="$TOOLCHAIN/bin/arm-openwrt-linux-uclibcgnueabi-g++"
export AR="$TOOLCHAIN/bin/arm-openwrt-linux-uclibcgnueabi-ar"
export CC_host="gcc"
export CXX_host="g++"
cd $OPENWRT_SDK
svn export svn://svn.openwrt.org/openwrt/branches/packages_12.09/libs/expat package/expat; make
cp build_dir/target-arm_mpcore+vfp_uClibc-0.9.33.2_eabi/expat-2.0.1/.libs/libexpat.a $TOOLCHAIN/lib
cp build_dir/target-arm_mpcore+vfp_uClibc-0.9.33.2_eabi/expat-2.0.1/lib/*.h $TOOLCHAIN/include/
cp -r /usr/include/X11 $TOOLCHAIN/include/
cd ../../
```    
## Some miscellaneous tricks to get things working  

11. Comment out the "latebindingsymbol" source in trunk/talk/libjingle.gyp so no ninja files are created for it.
```python
 ['os_posix==1', {
          'sources': [
            #'base/latebindingsymboltable.cc',
            #'base/latebindingsymboltable.h',
            'base/posix.cc',
            'base/posix.h',
            'base/unixfilesystem.cc',
            'base/unixfilesystem.h',
          ],
```
12. In trunk/thirdparty/openssl/openssl.gypi comment out the below  
```python
 #'openssl/crypto/chacha/chacha_vec_arm.S',
 #'openssl/crypto/chacha/chacha_vec.c',
```   
## Build ninja files  
 
13. In trunk directory run below command to generate ninja files. it should throw no errors.
```bash
gclient runhooks --force
```  
14. get sum custom ninja and definition files for ipop.  
```bash
wget https://github.com/pstjuste/ipop-tincan/raw/75e2c5fae7b6375ef2ead4a93595275492a6a259/build/typedefs.h
wget https://github.com/pstjuste/ipop-tincan/raw/75e2c5fae7b6375ef2ead4a93595275492a6a259/build/ipop-tincan.ninja
mv typedefs.h webrtc/typedefs.h
mv ipop-tincan.ninja out/Release/obj/talk/ipop-tincan.ninja
```   
15. Now we have to set some required compiler flags, from trunk directory execute below--
```bash
sed -i 's/fstack-protector/fno-stack-protector/g' `find out/Release -name *.ninja`
sed -i 's/fstack-protector/fno-stack-protector/g' `find out/Debug -name *.ninja`
find out/Release -type f -name *.ninja -exec sed -i 's/mfloat-abi=softfp/mfloat-abi=hard/g' {} +
find out/Debug -type f -name *.ninja -exec sed -i 's/mfloat-abi=softfp/mfloat-abi=hard/g' {} +
```   
## Build the executable binary  
 
16. Build the code using below command from trunk directory, you will encounter the below error
```bash
aumitra@ipop1-ThinkPad-T520:trunk$ ninja -C out/Release ipop-tincan
ninja: Entering directory `out/Release'
ninja: warning: multiple rules generate icudtl.dat. builds involving this target will not be correct; continuing anyway
[792/792] LINK ipop-tincan
FAILED: /home/saumitra/laguna_ipop/trunk/third_party/OpenWrt-SDK-cns3xxx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2/staging_dir/toolchain-arm_mpcore+vfp_gcc-4.8-linaro_uClibc-0.9.33.2_eabi/bin/arm-openwrt-linux-uclibcgnueabi-g++ -Wl,--fatal-warnings -Wl,-z,now -Wl,-z,relro -pthread -Wl,-z,noexecstack -fPIC -Wl,-O1 -Wl,--as-needed -Wl,--gc-sections -o ipop-tincan -Wl,--start-group obj/talk/ipop-project/ipop-tincan/src/ipop-tincan.tincan.o obj/talk/ipop-project/ipop-tincan/src/ipop-tincan.tincanconnectionmanager.o obj/talk/ipop-project/ipop-tincan/src/ipop-tincan.xmppnetwork.o obj/talk/ipop-project/ipop-tincan/src/ipop-tincan.controlleraccess.o obj/talk/ipop-project/ipop-tincan/src/ipop-tincan.tincanxmppsocket.o obj/talk/ipop-project/ipop-tincan/src/ipop-tincan.tincan_utils.o obj/talk/xmpp/ipop-tincan.jingleinfotask.o obj/third_party/openssl/libopenssl.a obj/talk/libjingle_p2p.a obj/third_party/jsoncpp/libjsoncpp.a obj/talk/libipop-tap.a obj/third_party/libsrtp/libsrtp.a obj/talk/libjingle.a  -Wl,--end-group -ldl -lrt -lexpat
obj/third_party/openssl/openssl/crypto/chacha/openssl.chacha_enc.o: In function `CRYPTO_chacha_20':
chacha_enc.c:(.text.CRYPTO_chacha_20+0x680): undefined reference to `CRYPTO_chacha_20_neon'
collect2: error: ld returned 1 exit status
ninja: build stopped: subcommand failed.
saumitra@ipop1-ThinkPad-T520:trunk$ 
```  
This error tells us that a method "CRYPTO_chacha_20" in file "chacha_enc.c" calls some method that was defined in the files that were commented out and not buld.  
  
17. We go out to "trunk/third_party/openssl/openssl/crypto/chacha/chacha_enc.c" and comment out the below line.  
```python
#if __arm__
	if (CRYPTO_is_NEON_capable() &&
	    ((intptr_t)in & 15) == 0 &&
	    ((intptr_t)out & 15) == 0)
		{
		//CRYPTO_chacha_20_neon(out, in, in_len, key, nonce, counter);
		return;
		}
#endif
```  
18. Run "ninja -C out/Release ipop-tincan" again to generate the binary in out/Release
```bash
saumitra@ipop1-ThinkPad-T520:trunk$ ninja -C out/Release ipop-tincan
ninja: Entering directory `out/Release'
ninja: warning: multiple rules generate icudtl.dat. builds involving this target will not be correct; continuing anyway
[3/3] LINK ipop-tincan
saumitra@ipop1-ThinkPad-T520:trunk$ 
```