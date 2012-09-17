---
layout: page
title : Tutorial 1 - Hello Awesomium
group: Tutorials

---

In this tutorial, we'll load a remote web-page into a WebView and save the result to a JPEG. This should give you a rough idea of how to use the native API.

### Before We Begin

Make sure you've read the [Getting Started](http://wiki.awesomium.com/getting-started/) articles.

### Tutorial

#### Start With This Code

Create a new file called __main.cc__ and add the following code to it:

{% highlight cpp %}
// Various included headers
#include <Awesomium/WebCore.h>
#include <Awesomium/BitmapSurface.h>
#include <Awesomium/STLHelpers.h>

// Various macro definitions
#define WIDTH   800
#define HEIGHT  600
#define URL     "http://www.google.com"

using namespace Awesomium;

// Our main program
int main() {

  // Your code goes here.
  
  return 0;
}
{% endhighlight %}


#### Initialize the Framework

The first thing we need to do in our program is initialize the WebCore. The WebCore singleton is resposible for creating views, loading resources, and dispatching events.

Let's initialize the WebCore using the default configuration settings, add the following to your `main()`:

{% highlight cpp %}
// Create the WebCore singleton with default configuration
WebCore* web_core = WebCore::Initialize(WebConfig());
{% endhighlight %}

#### Create the WebView

Now we can create a WebView (similar to a tab in Chrome) using the WebCore we just created. Let's create a WebView with the default settings (offscreen rendering surface, all session data will be stored in memory):

{% highlight cpp %}
// Create a new WebView instance with a certain width and height
WebView* view = web_core->CreateWebView(WIDTH, HEIGHT);
{% endhighlight %}

#### Load a Page

##### Wait Until the Page Has Finished Loading

#### Save the Rendered Page to a JPEG

#### Clean Up

### Further Reading