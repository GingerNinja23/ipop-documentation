Tested on Windows 7.

These instructions are derived from these links:

* https://sites.google.com/a/chromium.org/dev/developers/how-tos/install-depot-tools
* http://dev.chromium.org/developers/how-tos/build-instructions-windows
* https://sites.google.com/site/webrtc/reference/getting-started/prerequisite-sw
* https://sites.google.com/site/webrtc/reference/getting-started

## Download and install depot_tools

1.  Install [MS Visual C++ 2013 Redistributable](http://www.microsoft.com/en-us/download/details.aspx?id=40784)

1.  Install [MS Visual Studio Express 2013 for Desktop](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-desktop)

1.  Download [depot_tools for windows](https://src.chromium.org/svn/trunk/tools/depot_tools.zip)

2.  Right-click on **depot_tools.zip** and click on **extract all**.

3.  Add depot_tools folder to your environmental variables PATH

    ```
    set PATH=%PATH%;<path-to-depot_tools-directory>\depot_tools
    ```

4.  Open a cmd shell and run `gclient` which will automatically install other dependencies.
    You need to run gclient command twice and do not worry about errors that you see.

    ```
    set PATH=%PATH%;<path-to-depot_tools-directory>\depot_tools
    gclient --version
    gclient --version
    git config --global core.autocrlf false
    git config --global core.filemode false
    git config --global branch.autosetuprebase always
    ```

## Download source code

1.  Open a windows cmd shell and create libjingle folder

    ```
    mkdir libjingle
    cd libjingle
    ```
2. Set necessary environmental variables

    ```
    set DEPOT_TOOLS_WIN_TOOLCHAIN=0
    set GYP_GENERATORS=ninja
    set GYP_MSVS_VERSION=2013e
    # for Visual Studio Pro set GYP_MSVS_VERSION=2013
    ```

2.  Configure gclient to download libjingle code

    ```
    gclient config --name=trunk http://webrtc.googlecode.com/svn/branches/3.52
    ```

3.  Download libjingle and dependencies (this may take a while). Running this command twice
    ensures you have all dependencies (I think)

    ```
    gclient sync --force
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
    del all.gyp talk\libjingle.gyp
    copy talk\ipop-project\ipop-tincan\build\ipop-tincan.gyp talk\
    copy talk\ipop-project\ipop-tincan\build\libjingle.gyp talk\
    copy talk\ipop-project\ipop-tincan\build\all.gyp .
    ```

## Get additional dependencies

1.  Download and extract [pthreads-win32](ftp://sourceware.org/pub/pthreads-win32/pthreads-w32-2-9-1-release.zip) and move the Pre-built.2 folder into the third party directory

    ```
    move Pre-built.2\ <path-to-libjingle>\trunk\third_party\pthreads_win32
    ```

2. Download the [IPOP-tap Win32 dll](https://github.com/ipop-project/downloads/releases/download/ipop-dll-v15.01.01/ipop-dll.zip) and 
   extract it to ipop-dll and move the contents of to bin folder under ipop-tap directory

    ```
    mkdir <path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin
    move ipop-dll\ipoptap.dll <path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin
    move ipop-dll\ipoptap.def <path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin
    ```

3.  (Optional) If you would like to build your own IPOP-Tap dll, you have to cross-compile IPOP-Tap
    for Windows on Ubuntu (or Debian) using mingw with the following command from Ubuntu shell.
    You do not need to perform this step if you decide to use the prebuilt DLL from step two.
    By running the commands below, `make win32_dll` will create a bin folder, you need to copy the
    bin folder to the ipop-tap directory to your Windows machine and place it under the
    `<path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin` directory.

    ```bash
    # perform these commands on an Ubuntu or Debian machine

    sudo apt-get install gcc-mingw-w64-i686 unzip
    wget ftp://sourceware.org/pub/pthreads-win32/pthreads-w32-2-9-1-release.zip
    unzip pthreads-w32-2-9-1-release.zip
    cp Pre-built.2/include/* /usr/lib/gcc/i686-w64-mingw32/4.8/include
    cp Pre-built.2/lib/x86/* /usr/lib/gcc/i686-w64-mingw32/4.8/
    git clone --depth 1 https://github.com/ipop-project/ipop-tap.git
    cd ipop-tap
    make win32_dll
    ```

4.  Navigate to the IPOP-Tap bin folder and run lib tool which will create ipoptab.lib for
    Visual C++ to use

    ```
    cd <path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin
    "c:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\lib.exe" /def:ipoptap.def
    ```

## Building the code

1.  Update `<path-to-libjingle>\trunk\build\common.gypi` file to disable warning as error for
    the following warnings numbers: 4067, 4996. You need to open the file in your editor and
    search for _msvs_disabled_warnings_. It should look like the following

    ```python
    # line 2558
    'msvs_disabled_warnings': [4800,4996,4067],

    # line 2576
            'msvs_disabled_warnings': [
              4251, 4996, 4067  # class 'std::xx' needs to have dll-interface.
            ],


    # line 2626
            'msvs_disabled_warnings': [
              4251, 4996, 4067  # class 'std::xx' needs to have dll-interface.
            ],


    # line 4580
        'msvs_disabled_warnings': [4351, 4355, 4396, 4503, 4819,
          # TODO(maruel): These warnings are level 4. They will be slowly
          # removed as code is fixed.
          4100, 4121, 4125, 4127, 4130, 4131, 4189, 4201, 4238, 4244, 4245,
          4310, 4428, 4481, 4505, 4510, 4512, 4530, 4610, 4611, 4701, 4702,
          4706, 4996, 4067
        ],
    ```

1.  Generate Visual Studio solution files

    ```
    gclient runhooks --force
    ```
1.  Open cmd and go back to libjingle trunk directory and compile with ninja

    ```
    cd <path-to-libjingle>\trunk
    ninja -C out/Release ipop-tincan
    ```


2.  In order to run `ipop-tincan.exe`, you need to make sure to copy the following files to the
    same directory as `ipop-tincan.exe`

    * ipop-tincan.exe located under `<path-to-libjingle>\trunk\out\Release
    * ipoptap.dll located in `<path-to-libjingle>\trunk\talk\ipop-project\ipop-tap\bin`
    * pthreadGC2.dll and pthreadVC2.dll located in
      `<path-to-libjingle>\trunk\third_party\pthreads_win32\bin`

## Building IPOP Service

Building the code is a fairly straightforward process. At this time, it requires Git and MS Visual Studio 2013. The solution and project files are included alongside the source files. 

1. Clone the project to your local filesystem.

 ```
git clone --depth 1 https://github.com/ipop-project/ipop-service.git
 ```
2. Open the _IPoPSvc.sln_ file in Visual Studio 2013.
3. Select your build preference [Debug|Release].
4. Choose Build Solution from the Build menu. The executable will be created in appropriate [Debug|Release] subfolder.
5. Enjoy!

## Download controllers

1.  Download SocialVPN and GroupVPN controllers from [controllers repo](http://github.com/ipop-project/controllers/)