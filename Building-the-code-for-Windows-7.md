Tested on Windows 7 (32-bit).

These instructions are derived from these links:

* https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
* http://dev.chromium.org/developers/how-tos/build-instructions-windows
* https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
* https://sites.google.com/site/webrtc/reference/getting-started

## Download and install depot_tools

1.  Download [depot_tools for windows](https://src.chromium.org/svn/trunk/tools/depot_tools.zip)

2.  Right-click on **depot_tools.zip** and click on **extract all**.

3.  Add depot_tools folder to your environmental PATH

    1. Control Panel > User Accounts > User Accounts > Change my environment variables
    2. Add a PATH user variable: *%PATH%;C:\path\to\depot_tools*

4.  Open a cmd shell and run `gclient` which will automatically install other dependencies

## Install Visual C++ 2010 Express and SDKs

1.  Download and install [Visual C++ 2010](http://www.visualstudio.com/en-us/downloads#d-2010-express)

2.  Download and install [Visual Studio 2010 Service Pack](https://www.microsoft.com/en-us/download/details.aspx?id=23691)

3.  Download and install [Windows SDK 7](http://www.microsoft.com/en-us/download/details.aspx?id=8279)

4.  Download and install [Visual C++ SP1 Compiler Update](http://www.microsoft.com/en-us/download/details.aspx?id=4422)

5.  Download and install [Windows Driver Kit 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=11800)

## Download source code

1.  Open a windows cmd shell and create libjingle folder

    ```
    mkdir libjingle; cd libjingle
    ```

2.  Configure gclient to download libjingle code

    ```
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.46
    ```

3.  Download libjingle and dependencies (this may take a while).

    ```
    gclient sync --force
    ```

4.  Download ipop-tincan from github.com/ipop-project

    ```
    cd trunk\talk
    mkdir ipop-project
    cd ipop-project
    git clone --depth 1 https://github.com/ipop-project/ipop-tap.git
    git clone --depth 1 https://github.com/ipop-project/ipop-tincan.git
    ```

5.  Return to libjingle trunk directory

    ```
    cd ..\..
    ```

6.  Copy modified gyp files to trunk/talk directory

    ```
    copy talk\ipop-project\ipop-tincan\build\ipop-tincan.gyp talk\
    copy talk\ipop-project\ipop-tincan\build\libjingle.gyp talk\
    copy talk\ipop-project\ipop-tincan\build\all.gyp .
    ```

7.  Generate Visual Studio solution files

    ```
    gclient runhooks --force
    ```

## Get additional dependencies

1.  Download and extract [pthreads-win32](ftp://sourceware.org/pub/pthreads-win32/pthreads-w32-2-9-1-release.zip) and move the Pre-built.2 folder into the third party directory

    ```
    move Pre-built.2\ <path-to-libjingle>\trunk\third_party\pthreads_win32
    ```

2.  At the moment, you have to cross-compile IPOP-Tap for Windows on Ubuntu (or Debian)
    using mingw with the following command from Ubuntu shell

    ```bash
    sudo apt-get install gcc-mingw-w64-i686
    git clone --depth 1 https://github.com/ipop-project/ipop-tap.git
    cd ipop-tap
    make win32_dll
    ```

3.  This will create a bin folder, you need to copy the bin folder to the ipop-tap directory on Windows

4.  On Windows, open a **Visual Studio Command Prompt** by typing that in the run box

5.  Navigate to the IPOP-Tap bin folder and run lib tool which will create ipoptab.lib for
    Visual C++ to use

    ```
    cd <path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin
    lib /DEF:ipoptap.def
    ```

## Building with Visual Studio

1.  Use the file explorer and navigate to the libjingle trunk directory

2.  You will see a file called **all.sln**, double-click on it. This will open the project
    on Visual Studio

3.  Once it is loaded, look for **ipop-tincan** in the list of project in the solution explorer.

4.  Right click on **ipop-tincan** and click on Build, and it will take some time to build

5.  Once build is complete, you will find the binary under
    `<path-to-libjingle>\trunk\build\Debug\ipop-tincan.exe`

6.  In order to run `ipop-tincan.exe`, you need to make sure to copy the following files to the
    same directory as `ipop-tincan.exe`

    * ipoptap.dll located in `<path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin`
    * pthreadGC2.dll and pthreadVC2.dll located in
      `<path-to-libjingle>\trunk\third_party\pthreads_win32\bin`

## Download controllers

1.  Download socialvpn and groupvpn controllers

    ```
    wget http://github.com/ipop-project/socialvpn/raw/master/src/svpn_controller.py
    wget http://github.com/ipop-project/groupvpn/raw/master/src/gvpn_controller.py
    ````