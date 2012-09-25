---
layout: page
title : Introduction to Web-Views
group: General Use
weight: 2

---

### What is a WebView?

A WebView is like a tab in a browser. You load pages into a WebView, interact with it, and display it in some pre-defined manner.

There are two different types of WebViews:

#### Offscreen Views

Offscreen Views are rendered continuously to a pixel-buffer surface (see `WebView::surface()`). It is your responsibility to display this surface in your application and pass all mouse/keyboard events. This gives you the flexibility to display web-content in any environment (such as a 3D engine or headless renderer).

All WebViews are Offscreen by default.

#### Windowed Views

Windowed Views are rendered directly to a platform window (HWND, NSView, etc.) and capture all input themselves.

On MS Windows, you should call `WebView::set_parent_window` immediately after creating this type of WebView (the view cannot create a window until a parent is set).

On Mac OSX, you will need to retrieve  `WebView::window` after creating this type of WebView and add it to your own container to display the view in your application.

### Creating a WebView

You create WebViews using the WebCore, here's an example:

{% highlight cpp %}
// Create an offscreen WebView
WebView* view = web_core->CreateWebView(500, 500);
{% endhighlight %}

Here's an example of how to create a windowed WebView on MS Windows:

{% highlight cpp %}
// Create a windowed WebView
WebView* view = web_core->CreateWebView(500, 500, kWebViewType_Window);
view->set_parent_window(parent_hwnd);
{% endhighlight %}

### Loading Content

### Registering Listeners

### Cleaning Up