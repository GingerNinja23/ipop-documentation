These instructions are from these links:

-   https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
-   https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
-   https://sites.google.com/site/webrtc/reference/getting-started

## Installing tools and code

### Install Java

Our code does not need Java to compile or run, but the setup tools require it.

On Debian, you can simply install the `default-jdk`:

```bash
sudo apt-get install default-jdk
```

And then set the `JAVA_HOME` environment variable:

```bash
echo 'export JAVA_HOME=/usr/lib/jvm/default-java' >> ~/.profile
export JAVA_HOME=/usr/lib/jvm/default-java
```

### Install necessary libraries and chromium tools

1.  This works on Debian-based distros

    ```bash
    sudo apt-get install libexpat1-dev git subversion build-essential
    ```

2.  Download depot_tools for chromium repo

    ```bash
    git clone --depth 1 -b master https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```

3.  Set up environmental variables

    ```bash
    export PATH="$(pwd)/depot_tools:$PATH"
    ```

### Get the libjingle and ipop-tincan source code

1.  Configure gclient to download libjingle code

    ```bash
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.44
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
    cp talk/ipop-project/ipop-tincan/build/libjingle.gyp talk/
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