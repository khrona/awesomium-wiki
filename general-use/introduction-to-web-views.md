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

The primary way to load content into a WebView is via `WebView::LoadURL`. For example, to begin navigating a WebView to Google, you would call:

{% highlight cpp %}
WebURL url(WSLit("http://www.google.com"));
view->LoadURL(url);
{% endhighlight %}

All URLs should be properly formatted before passing it to LoadURL. You can check if a URL is valid via `WebURL::IsValid`:

{% highlight cpp %}
WebURL url(some_url_string);
if (!url.IsValid())
  std::cerr << "Error! URL was unable to be parsed.";
{% endhighlight %}

LoadURL makes it really easy to load remote content on the Internet, but what about local resources? 

#### Defining a Custom DataSource

If you would like to load local resources with your application we recommend using DataSource. This powerful bit of API allows you to provide a custom resource loader for a set of URLs that match a certain prefix.

See [this article](using-data-sources.html) for more information on using DataSource.

### Registering Listeners

The WebViewListener namespace contains a suite of special classes that you can use to respond to certain events in a WebView.

For example, to listen for all load-related events, you would simply make your own subclass of WebViewListener::Load and then register an instance of it to a certain WebView using `WebView::set_load_listener`. Here's a psuedo-example:

{% highlight cpp %}
// Define our WebViewListener::Load subclass
class MyLoadListener : public Awesomium::WebViewListener::Load {
  // ... All the overriden WebViewListener::Load methods go here
};

// Create an instance of MyLoadListener and register it to a certain WebView
MyLoadListener* my_load_listener = new MyLoadListener();
my_web_view->set_load_listener(my_load_listener);
{% endhighlight %}

Please note that all WebViewListener events are dispatched asynchronously (meaning that the event may arrive a few milliseconds after the event actually happened in the child-process).

### Cleaning Up