Tested on Ubuntu 12.04 and Debian Wheezy.

Go on the [[Building the code for Linux page|Building the code for Linux]]
and follow the instructions in the first two sections:

* Download dependencies
* Download source code

## Download Android dependencies

1.  Return to libjingle root directory, update .gclient file to download Android dependencies

    ```bash
    cd ../../../
    echo "target_os = ['android', 'unix']" >> .gclient
    gclient sync --force
    ```

2.  Set up android environmental variables (ignore message asking for Oracle Java)

    ```bash
    cd trunk
    source build/android/envsetup.sh
    ```

## Build ipop-tincan for Android

1.  Create ninja build files

    ```bash
    gclient runhooks --force
    ```

2.  Build tincan for android (binary located at out/Release/ipop-tincan)

    ```bash
    ninja -C out/Release ipop-tincan
    ```

3.  To build debug version with gdb symbols (but creates 25 MB binary)

    ```bash
    ninja -C out/Debug ipop-tincan
    ```

4.  The generated binary is located at `out/Release/ipop-tincan` or
    `out/Debug/ipop-tincan`
