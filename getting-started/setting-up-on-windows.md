---
layout: page
title : Setting Up on Windows
group: Getting Started
weight: 1

---

### Before We Begin

This tutorial assumes that you are developing a C++ application. If you are a developing a .NET/C# application please see our [.NET Wiki](http://wiki.awesomium.net/getting-started/) instead.

### Checklist
You will need the following:

* The latest Awesomium v1.7 SDK for Windows (download it [here](http://www.awesomium.com/download/))
* Microsoft Visual C++ (there is a free 'Express' version)

### Install the SDK
After downloading the SDK, unzip the ZIP file to some convenient location.

#### Folder Structure of the SDK
The SDK should contain the following folders in the installation directory:

* __build/bin__ folder: Contains DLLs that you will need to bundle with your application
* __build/lib__ folder: Contains static libraries that you will need to link against your application.
* __include__ folder: Contains headers that you will need to #include in your source files to access our C++ API

### Set up your project
#### Configure project settings

1. Create a new project in MSVC (the __Empty Project__ template is recommended).
2. Add your C++ source files.
3. Right-click your project name, select __Properties__ to open up the __Property Pages__ dialog.
4. Add the path to the SDK's __include__ folder to __Additional Include Directories__ (should be under Configuration Properties -> C/C++ -> General) for both Debug and Release configurations. 
5. Add the paths to the SDK's __lib__ directory (e.g., build/lib) to __Additional Library Directories__ (should be under Configuration Properties -> Linker -> General) for all build configurations. 
6. Add __awesomium.lib__ to __Additional Dependencies__ (should be under Configuration Properties -> Linker -> Input) for all build configurations.

#### Copy files to your build distribution
Before running your executable, __make sure to copy the following files__ from the SDK's __build/bin__ directory to your respective build directories. 

 * `avcodec-53.dll`
 * `avformat-53.dll`
 * `avutil-51.dll`
 * `awesomium.dll`
 * `awesomium_process.exe`
 * `icudt.dll`
 * `libEGL.dll`
 * `libGLESv2.dll`
 * `xinput9_1_0.dll`
 
If you wish to use the remote inspector (see `WebConfig`), make sure to also copy `inspector.pak` to your working directory.

### Include the API
To include the entire API for Awesomium in your source files, you simply need to include WebCore.h:

    #include <Awesomium/WebCore.h>
   
### Read some more articles
* [Basic Concepts](basic-concepts.html)


