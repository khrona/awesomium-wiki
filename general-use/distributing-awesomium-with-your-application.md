---
layout: page
title : Distributing Awesomium With Your Application
group: General Use
weight: 6
---

### What files should I include with my application?

You should always include the following files from the SDK in your app's working directory:

 * awesomium.dll
 * awesomium_process.exe
 * avcodec-53.dll
 * avformat-53.dll
 * avutil-51.dll
 * icudt.dll
 * libEGL.dll
 * libGLESv2.dll

The following files are optional:

 * inspector.pak (Only needed if you enable the remote inspector)
 * awesomium_pak_utility.exe (Utility for creating PAK files to use with DataPakSource)
 * awesomium_symbols.pdb (Debug Symbols for awesomium.dll)
 * awesomium_process.pdb (Debug Symbols for awesomium_process.exe)
 
### What about Flash?

If you need to use Flash in your application, you should bundle the Flash installer (the NPAPI build, usually for Firefox) with your application.