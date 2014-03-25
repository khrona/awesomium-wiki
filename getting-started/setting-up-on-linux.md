---
layout: page
title : Setting Up on Linux
group: Getting Started
weight: 3

---

### Before We Begin

This tutorial assumes that you are developing a C++ application. If you are a developing a .NET/C# application please see our [.NET Wiki](http://wiki.awesomium.net/getting-started/) instead.

### Checklist

Before we begin, you'll need the following:

*    The **Awesomium SDK** (download it [here](http://www.awesomium.com/download))
*    Ubuntu Linux

### Install the SDK

 1. After downloading the SDK, extract it to a folder in your home directory.
 2. Open the Terminal.
 3. Change directories to the SDK files you just extracted.
 4. Run `sudo make all` to install the library to your machine.

### Run the Samples

You should now be able to run the samples. Change directories to the `./bin` folder and run some samples:

The Hello World example loads up Google and renders it to a JPG on disk:

    ./awesomium_sample_hello
    
The WebFlow sample depends on SDL 1.2 and OpenGL, demonstrates a simple 3D browser:

    ./awesomium_sample_webflow
    
### Using Awesomium

To use most of the Awesomium API in your C++ code, just include the following:

    #include <Awesomium/WebCore.h>
    
To link against Awesomium 1.7.4 in your applications, just add "-lawesomium-1-7"

    g++ main.cpp -lawesomium-1-7
    
   
### Read some more articles
* [Basic Concepts](basic-concepts.html)
