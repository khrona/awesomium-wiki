---
layout: page
title : Setting Up on Mac OSX
group: Getting Started
weight: 2

---

### Before We Begin

This tutorial assumes that you are developing a C++ Application. If you want to use Awesomium with .NET or C#, then you'll need to follow our Awesomium.NET tutorial instead.

### Checklist

Before we begin, you'll need the following:

*    The latest Awesomium 1.7 SDK for Mac OSX (download it [here](http://www.awesomium.com/download))
*    An Intel Mac running Mac OS X 10.6 (“Snow Leopard”) or higher.
*    XCode  3.1.2 or higher

### Install the SDK

After downloading the SDK, open up the DMG file (or, if you're using an experimental build, the ZIP file) and accept any license agreements that are displayed.

Inside the distribution, you should find a build of **Awesomium.framework**.

Copy Awesomium.framework to some folder on your hard drive-- just remember the path to the directory, you'll need it later.

### Set up your project

#### Configure project settings

1.    Create a new project in XCode (either a Cocoa Application or Command Line Tool).
1.    Double click your project to open Project Info and select the **Build** tab.
    1.    Select **All Configurations** in the **Configuration** drop-down list.
    1.    Under the **Architectures** property, make sure **32-bit Universal** is selected.
    1.    Under the **Valid Architectures** property, remove all architectures except **i386**.
    1.    Under the **Framework Search Paths** property (you may need to scroll down a bit to find it), add the path that contains the Awesomium.framework you copied earlier.

#### Link against the framework

To use Awesomium in your Mac OSX application, you will need to link against the Awesomium framework.

There are actually a couple of ways you can do this:

*    Drag and drop **Awesomium.framework** onto the name of your project in XCode's **Groups & Files** panel. In the dialog that appears, make sure your project is selected under **Add To Targets**.
*    OR double-click the name of your Application under the **Targets** list in XCode's **Groups & Files** panel. Select the **General** tab and then click the **+** symbol under **Linked Libraries**. Click **Add Other** at the bottom of the dialog that appears and then browse to and select **Awesomium.framework**.

#### Copy the framework to your build distribution

##### If your project is using an Application Bundle (e.g., you're creating a **Cocoa Application**):

1.    Right-click your Application's name under **Targets** in XCode's **Groupes & Files** panel. 
1.    Select Add -> New Build Phase -> New Copy Files Build Phase.
1.    In the dialog that appears, select **Frameworks** under the **Destination** drop-down list.
1.    Close the dialog.
1.    Expand your Application's list of build phases (just click the little triangle next to your Application's name under **Targets** to expand the list).
1.    Drag and drop **Awesomium.framework** onto the **Copy Files** build phase that you just created (should be the one at the bottom of the list).
1.    The Awesomium framework should automatically be copied into your Application Bundle every time you build your application.

##### If your project is NOT using an Application Bundle (e.g., you're creating a **Shell Tool**):

1.    Create a folder named **Frameworks** in the directory *above* the directory that contains your built executable (this will usually end up being the `build` directory of your XCode project).
2.    Copy **Awesomium.framework** to the **Frameworks** folder.
3.    Your directory structure should look like the following:

    ¦
    +---SomeDirectory
    ¦   +---YourExecutable
    ¦
    +---Frameworks
        +---Awesomium.framework


### Include the API

To include the entire API for Awesomium in your source files, you simply need to include **WebCore.h**.

    #include <Awesomium/WebCore.h>

### Further Reading

*    [Basic Concepts](basic-concepts.html)


