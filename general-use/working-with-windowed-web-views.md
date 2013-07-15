---
layout: page
title : Working with Windowed Web-Views
group: General Use
weight: 4

---

### Windowed Web-Views

Windowed Web-Views are a new feature to the 1.7 branch that essentially allow you to render a WebView directly to a platform-specific window (such as a HWND on Windows).

These Web-Views capture mouse/keyboard input and usually must be attached to some kind of container to be displayed.

Rendering a WebView directly to a window can dramatically improve performance in situations where a WebView needs to be displayed in a conventional, 'popup' manner (as opposed to layered within a 3D engine or projected non-uniformaly onto a surface).

### Creating a Windowed Web-View

You can create a windowed Web-View via `WebCore::CreateWebView`. Here's an example of how to create such a view on the Windows platform

{% highlight cpp %}
// Create a windowed WebView with initial size of 500 x 500
WebView* view = web_core->CreateWebView(500, 500, 0, kWebViewType_Window);

// Attach the window to our parent window container
view->set_parent_window(parent_hwnd);
{% endhighlight %}