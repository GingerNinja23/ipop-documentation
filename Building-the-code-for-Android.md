Go on the Building the code for Linux page (https://github.com/ipop-project/ipop-tincan/wiki/Building-the-code-for-Linux) and follow the instructions for the following sections

* Install Java
* Install necessary libraries and chromium tools
* Get the libjingle and ipop-tincan source code

### Set up Android build environment and build code

1.  Return to libjingle root directory, update .gclient file and download Android dependencies

    ```bash
    cd ../../../
    echo "target_os = ['android', 'unix']" >> .gclient
    gclient sync --force
    ```
2.  Set up android environmental variables (ignore message asking for Oracle Java)

    ```bash
    cd trunk
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