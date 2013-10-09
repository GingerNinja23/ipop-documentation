These instructions are from these links:

-   https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
-   https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
-   https://sites.google.com/site/webrtc/reference/getting-started

## Installing tools and code

### Install Oracle Java6

Our code does not need Java to compile or run, but the setup tools require it.

Download [Java 6][] and accept the license agreement. Click on
`jdk-6u45-linux-x64.bin` for 64bit Linux (or `jdk-6u45-linux-i586.bin` for 32bit
Linux) to download.

  [Java 6]: http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html#jdk-6u45-oth-JPR

1.  Do the following to extract Java

    ```bash
    chmod 755 jdk-6u45-linux-x64.bin
    ./jdk-6u45-linux-x64.bin
    ```

### Install necessary libraries and chromium tools

1.  This works on Debian-based distros

    ```bash
    sudo apt-get install libnss3-dev libasound2-dev libpulse-dev libjpeg62-dev \ 
                         libxv-dev libgtk2.0-dev libexpat1-dev libssl-dev git \
                         subversion build-essential
    ```

2.  Download depot_tools for chromium repo

    ```bash
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```

3.  Set up environmental variables

    ```bash
    export JAVA_HOME="$(pwd)/jdk1.6.0_45"
    export PATH="$JAVA_HOME/bin:$(pwd)/depot_tools:$PATH"
    ```

### Get the libjingle and ipop-tincan source code

1.  Configure gclient to download libjingle code

    ```bash
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.44
    echo "target_os = ['android', 'unix']" >> .gclient
    ```

2.  Download libjingle and dependencies (this takes a while)

    ```bash
    gclient sync --force
    ```

3.  Download ipop-tincan from github.com/ipop-project

    ```bash
    cd trunk/talk; mkdir ipop-project; cd ipop-project
    git clone https://github.com/ipop-project/ipop-tap.git
    git clone https://github.com/ipop-project/ipop-tincan.git
    ```

## Building ipop-tincan

### For Linux

1.  Return to libjingle trunk directory

    ```bash
    cd ../../
    ```

2.  Copy modified gyp files to trunk/talk directory

    a.  Run:

        ```bash
        cp talk/ipop-project/ipop-tincan/build/ipop-tincan.gyp talk/
        cp talk/ipop-project/ipop-tincan/build/all.gyp .
        ```

    b.  (Optional) Add this step on a 32-bit machine

        ```bash
        export GYP_DEFINES="target_arch=ia32"
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

### For Android

1.  Assuming you have just compiled for Linux for instructions above, move
    binaries to avoid overwriting them

    ```bash
    mv out out.x86_64
    ```

2.  Set up android environmental variables

    ```bash
    source build/android/envsetup.sh
    export GYP_DEFINES="build_with_libjingle=1 build_with_chromium=0 \
                        libjingle_java=0 $GYP_DEFINES"
    ```

3.  Create ninja build files

    ```bash
    gclient runhooks --force
    ```

4.  Build tincan for android (binary located at out/Release/ipop-tincan)

    ```bash
    ninja -C out/Release ipop-tincan
    ```

5.  To build debug version with gdb symbols (but creates 25 MB binary)

    ```bash
    ninja -C out/Debug ipop-tincan
    ```
