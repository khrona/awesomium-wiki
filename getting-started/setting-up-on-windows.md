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
Run the installer and follow the on-screen instructions. Upon success, you should find a folder in your Start Menu with additional links and samples to play with.

#### Environment Path

The installer should install the includes and library files under a path which looks like the following:

`C:\Program Files (x86)\Awesomium Technologies LLC\Awesomium SDK\1.7.2.2`

To help you reference this path in your projects, an environment variable named `AWE_DIR` should have been defined during installation.

#### Folder Structure of the SDK
The SDK should contain the following folders in the installation directory:

* __build/bin__ folder: Contains DLLs that you will need to bundle with your application
* __build/bin/packed__ folder: Contains optional compressed DLLs to use with your application (smaller size, very slight delay in load time).
* __build/lib__ folder: Contains static libraries that you will need to link against your application.
* __include__ folder: Contains headers that you will need to #include in your source files to access our C++ API

### Set up your project
#### Configure project settings

1. Create a new project in Microsoft Visual C++ (the __Empty Project__ template is recommended).
2. Add your C++ source files.
3. Right-click your project name, select __Properties__ to open up the __Property Pages__ dialog.
4. Add `$(AWE_DIR)include` to __Additional Include Directories__ (should be under Configuration Properties &rarr; C/C++ &rarr; General) for both Debug and Release configurations. 
5. Add `$(AWE_DIR)build\lib` to __Additional Library Directories__ (should be under Configuration Properties &rarr; Linker &rarr; General) for all build configurations. 
6. Add `awesomium.lib` to __Additional Dependencies__ (should be under Configuration Properties &rarr; Linker &rarr; Input) for all build configurations.

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


