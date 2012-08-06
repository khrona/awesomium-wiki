---
layout: page
title : Basic Concepts
group: Getting Started
weight: 3

---
{% include JB/setup %}

There's only a few key concepts that you'll need to be familiar with to use Awesomium.

### The WebString

The first of these is the WebString. This class is used to represent UTF-16 strings throughout our C++ API, here's an example of how to create one:

{% highlight cpp %}
WebString my_string = WebString::CreateFromUTF8("Hello", strlen("Hello"));
size_t len = my_string.length(); // length is 5
{% endhighlight %}
 

#### Using WebString with the STL
We provide some additional convenience methods for converting to different string types inside the STLHelpers.h header. If your application uses the STL it's a good idea to include this header. Here's an example of use:

{% highlight cpp %}
// Initialize my_string with a C-string literal
WebString my_string(WSLit("Hello World!"));

// Print my_string to the console
std::cout << my_string << std::endl;

// Convert my_string to a std::string
std::string my_utf8_string = ToString(my_string);
{% endhighlight %}

### The WebCore
The WebCore is the "core" of Awesomium-- it manages the lifetime of all WebViews (more on that later) and maintains useful services like resource caching and network connections.

Generally, you should create an instance of the WebCore at the beginning of your program and then destroy the instance at the end of your program. Here's an example of how to create it:

{% highlight cpp %}
#include <Awesomium/WebCore.h>
#include <Awesomium/STLHelpers.h>

using namespace Awesomium;

int main() {
  WebCore* web_core = WebCore::Initialize(WebConfig());

  WebCore::Shutdown();

  return 0;
}
{% endhighlight %}

#### Configuring the WebCore
You can configure the WebCore with some global settings via the first parameter of its constructor. Here's an example with a custom log path:

{% highlight cpp %}
WebConfig config;
config.log_path = WSLit("C:\\My Documents\\My Custom Log Path");
config.log_level = kLogLevel_Normal;

WebCore* web_core = WebCore::Initialize(config);
{% endhighlight %}

#### Updating the WebCore
In your main program loop, make sure to call WebCore::update to give it a chance to do some behind-the-scenes work. Here's a psuedo-example:

{% highlight cpp %}
void UpdateApplication() {
  web_core->Update();
  // do other application stuff here
}
{% endhighlight %}

### The WebSession
A WebSession is responsible for storing all user-generated data (cookies, cache, authentication, etc.). It can either be purely in-memory or saved to disk (you will need to provide a writeable path to store the data at runtime).

You can create a WebSession like so:

{% highlight cpp %}
WebSession* my_session = web_core->CreateWebSession(WSLit("C:\\Session Data Path"), WebPreferences());
{% endhighlight %}

#### Specifying Custom Preferences
You can supply custom preferences for each WebSession via the second parameter. Here's an example:

{% highlight cpp %}
WebPreferences prefs;
prefs.enable_plugins = false;
prefs.enable_smooth_scrolling = true;
prefs.user_stylesheet = WSLit("* { background-color: yellow !important; }");

// Create an in-memory session (empty path) with our custom preferences
WebSession* my_session = web_core->CreateWebSession(WSLit(""), prefs);
{% endhighlight %}
 
#### Destroying the WebSession
It is your responsibility to call WebSession::Release once you are done using the WebSession. If you forget to do this we can't guarantee that your WebSession will be saved to disk upon application exit.

{% highlight cpp %}
WebSession* my_session = web_core->CreateWebSession(WSLit(""), WebPreferences());
  
my_session->Release();
{% endhighlight %}

#### The WebView
A WebView is like a tab in a browser. You load pages into a WebView, interact with it, and render it on-the-fly to a certain graphics surface. You create WebViews using the WebCore, here's an example:

{% highlight cpp %}
// Create a new WebView with a width and height of 500px
WebView* my_web_view = web_core->CreateWebView(500, 500);
{% endhighlight %}

#### Specifying a WebSession for each WebView
You can specify which WebSession to use for each WebView via the third parameter of WebCore::CreateWebView. If you specify NULL, a default in-memory WebSession will be used instead. Here's an example of use:

{% highlight cpp %}
WebSession* my_session = web_core->CreateWebSession(WSLit(""), WebPreferences());
WebView* my_web_view = web_core->CreateWebView(500, 500, my_session);
{% endhighlight %}

Using the above API you can have one or more WebSessions per application, and each WebSession can be used with one or more WebViews. This is useful for isolating WebViews from each other (eg, incognito mode in Chrome).
 
#### Displaying the WebView
It is your responsibility to display the WebView and pass it input within your application. Each WebView has an abstract Surface that you can retrieve via WebView::surface (this instance is owned by the WebView, you shouldn't destroy it):

{% highlight cpp %}
Surface* surface = my_web_view->surface();
{% endhighlight %}

By default, the underlying Surface is of type BitmapSurface (you can specify your own Surface implementation via WebCore::set_surface_factory). It is safe to cast this surface to BitmapSurface to access the pixels:

{% highlight cpp %}
#include <Awesomium/BitmapSurface.h>

BitmapSurface* surface = static_cast<BitmapSurface>(my_web_view->surface());
surface->SaveToJPEG(WSLit("C:\\bitmap.jpg"));
{% endhighlight %}

#### Passing mouse input
To interact with the WebView, you'll need to pass it mouse events. Here are some quick examples:

{% highlight cpp %}
// Inject a mouse-movement event to (75, 100) in screen-space (0,0 is top-left of screen)
my_web_view->InjectMouseMove(75, 100);

// Inject a left-mouse-button down event
my_web_view->InjectMouseDown(kMouseButton_Left);

// Inject a left-mouse-button up event
my_web_view->InjectMouseUp(kMouseButton_Left);
{% endhighlight %}

#### Passing keyboard input
To inject keyboard events into a WebView, you will need to create a WebKeyboardEvent and pass it to WebView::injectKeyboardEvent. You can either create a WebKeyboardEvent from a platform-specific keyboard event (eg, MSG, WPARAM, LPARAM on Windows) or synthesize one yourself. See "passing keyboard events" for more information.

#### Managing input focus
Any time you interact with a WebView, you should first make sure the WebView is focused by calling WebView::Focus. If you forget to do this, textboxes may not behave correctly (e.g., carets and selection indicators may not display).

Similarly, to remove focus from a WebView, you should call WebView::Unfocus.

#### Handling WebView Events
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