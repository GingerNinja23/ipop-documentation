Tested on Ubuntu 12.04 and Debian Wheezy.

These instructions are derived from these links:

* https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
* https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
* https://sites.google.com/site/webrtc/reference/getting-started

## Download dependencies

1.  Install dependencies

    ```bash
    sudo apt-get install default-jdk libexpat1-dev git subversion build-essential
    ```

2.  Download depot_tools for chromium repo

    ```bash
    mkdir libjingle; cd libjingle
    git clone --depth 1 https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```

3.  Set up environmental variables

    ```bash
    export JAVA_HOME=/usr/lib/jvm/default-java
    export PATH="$(pwd)/depot_tools:$PATH"
    ```

## Download source code

1.  Configure gclient to download libjingle code

    ```bash
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.46
    ```

2.  Download libjingle and dependencies (this may take a while)

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

