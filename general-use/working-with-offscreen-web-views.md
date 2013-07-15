---
layout: page
title : Working with Offscreen Web-Views
group: General Use
weight: 3

---

### Creating an Offscreen Web-View

You can create an offscreen Web-View via `WebCore::CreateWebView`. Here's an example of how to create such a view:

{% highlight cpp %}
// Create an offscreen WebView with initial size of 500 x 500
WebView* view = web_core->CreateWebView(500, 500, 0, kWebViewType_Offscreen);
{% endhighlight %}

### Displaying the Surface
It is your responsibility to display offscreen WebViews and pass it input within your application. Each WebView has an abstract Surface that you can retrieve via WebView::surface (this instance is owned by the WebView, you shouldn't destroy it):

{% highlight cpp %}
Surface* surface = my_web_view->surface();
{% endhighlight %}

By default, the underlying Surface is of type BitmapSurface (you can specify your own Surface implementation via WebCore::set_surface_factory). It is safe to cast this surface to BitmapSurface to access the pixels:

{% highlight cpp %}
#include <Awesomium/BitmapSurface.h>

BitmapSurface* surface = static_cast<BitmapSurface*>(my_web_view->surface());
surface->SaveToJPEG(WSLit("C:\\bitmap.jpg"));
{% endhighlight %}

#### Defining Your Own SurfaceFactory

If you want to intercept Paint/Scroll-Pixel events directly (eg., for writing to an OpenGL surface or streaming to a compressed video format), you can define your own Surface implementation via SurfaceFactory.

First you'll need to define your own Surface implementation by sub-classing Surface.

Then, you should define your own SurfaceFactory implementation that creates and destroys an instance of your custom Surface class.

Next, you should register your SurfaceFactory with `WebCore::set_surface_factory`.

Now, all WebViews should use your SurfaceFactory to create Surfaces, you should cast `WebView::surface()` to your Surface's type to interact with it.

### Passing mouse input
To interact with the WebView, you'll need to pass it mouse events. Here are some quick examples:

{% highlight cpp %}
// Inject a mouse-movement event to (75, 100) in screen-space (0,0 is top-left of screen)
my_web_view->InjectMouseMove(75, 100);

// Inject a left-mouse-button down event
my_web_view->InjectMouseDown(kMouseButton_Left);

// Inject a left-mouse-button up event
my_web_view->InjectMouseUp(kMouseButton_Left);
{% endhighlight %}

### Passing keyboard input
To inject keyboard events into a WebView, you will need to create a WebKeyboardEvent and pass it to `WebView::InjectKeyboardEvent`. You can either create a WebKeyboardEvent from a platform-specific keyboard event (eg, MSG, WPARAM, LPARAM on Windows) or synthesize one yourself. See "passing keyboard events" for more information.

### Managing input focus
Any time you interact with a WebView, you should first make sure the WebView is focused by calling `WebView::Focus`. If you forget to do this, textboxes may not behave correctly (e.g., carets and selection indicators may not display).

Similarly, to remove focus from a WebView, you should call `WebView::Unfocus`.