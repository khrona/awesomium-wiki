---
layout: page
title : Tutorial 2 - Displaying Your First Page
group: Tutorials
weight: 2

---

> This tutorial is under construction, check back soon.

In this tutorial, we'll load our own HTML into a WebView and display it within a window.

### Start With This Code

It's quite a bit of work to set up an empty Windows GDI or Mac OSX Cocoa application. To save some time we've gone ahead and done some of the work for you.

Start by downloading one of the following frameworks and following the README for your platform:

 * [Tutorial Framework for Awesomium (GitHub ZIP)](https://github.com/awesomium/tutorial-framework/archive/master.zip)

#### Windowed WebViews

It's important to note that we will be using Windowed WebViews (as opposed to Offscreen WebViews) with the Tutorial Framework. These views render directly to a platform-specific graphics surface and capture input as well.

### Loading Custom HTML

After setting up your project using the tutorial framework, open up `main.cc` and find the following code:

    // Inherited from Application::Listener
    virtual void OnLoaded() {
      view_ = View::Create(512, 512);
      view_->web_view()->LoadURL(WebURL(WSLit("http://www.google.com")));
      // < Set up your View here. >
    }


### Further Reading

