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

__See [this article](../general-use/using-web-sessions.html) for more information about WebSessions.__

### The WebView
A WebView is like a tab in a browser. You load pages into a WebView, interact with it, and render it on-the-fly to a certain graphics surface. You create WebViews using the WebCore, here's an example:

{% highlight cpp %}
// Create a new WebView with a width and height of 500px
WebView* my_web_view = web_core->CreateWebView(500, 500);
{% endhighlight %}

There are two types of WebViews: offscreen and windowed. __See [this article](../general-use/introduction-to-web-views.html) for more information about using each.__
 
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