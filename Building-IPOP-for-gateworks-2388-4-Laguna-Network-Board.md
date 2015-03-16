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
 
execute 'gclient sync --force again to download openssl and this time there will be no errors.  
