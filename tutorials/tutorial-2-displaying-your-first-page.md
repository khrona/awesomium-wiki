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

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(512, 512);
     // < Set up your View here. >
  }
{% endhighlight %}

The `OnLoaded` method is called within the tutorial framework once the application has finished launching. This is your best chance to set up your WebViews and make them do interesting things.

The framework uses a special wrapper class called `View` which consists of a platform window and a WebView. Notice the code above creates one View with dimensions of 512x512.

If you ran this code right now, nothing would be displayed because the WebView contains no content. Let's fix that.

Replace the `// <Set up your View here. >` code with the following:

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(512, 512);

    WebURL url(WSLit("data:text/html,<p>Hello World</p>"));
    view_->web_view()->LoadURL(url);
  }
{% endhighlight %}


### Further Reading

