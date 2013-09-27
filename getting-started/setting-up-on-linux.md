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

*    First, extract the SDK. For the purposes of this guide, we will be using the 32-bit version of Awesomium 1.7.2, but any version will do.

        tar -xzvf awesomium_1_7_2_sdk_linux32.tar.gz

*    Then, go to the directory and run `sudo make all`.

        cd awesomium_v1.7.2_sdk_linux32
        sudo make all

### Running the sample applications

*    Go into the `awesomium_v1.7.2_sdk_linux32/bin` directory, and then start the awesomium process.

        cd awesomium_v1.7.2_sdk_linux32/bin
        awesomium_process

*    Execute one of the samples.

        ./awesomium_sample_hello

_This article is still under construction, check back soon!_
