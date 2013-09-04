---
layout: page
title : Tutorial 1 - Hello Awesomium
group: Tutorials
weight: 1

---

> Make sure you've read the [Getting Started](http://wiki.awesomium.com/getting-started/) articles.

<p class="intro">In this tutorial, we'll load a remote page into a WebView and save the result to a JPEG.</p>
### Before We Begin

### Start With This Code

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


### Initialize the Framework

The first thing we need to do in our program is initialize the WebCore. The WebCore singleton is resposible for creating views, loading resources, and dispatching events.

Let's initialize the WebCore using the default configuration settings, add the following to your `main()`:

{% highlight cpp %}
// Create the WebCore singleton with default configuration
WebCore* web_core = WebCore::Initialize(WebConfig());
{% endhighlight %}

### Create the WebView

Now we can create a WebView (similar to a tab in Chrome) using the WebCore we just created. Let's create a WebView with the default settings (offscreen rendering surface, all session data will be stored in memory):

{% highlight cpp %}
// Create a new WebView instance with a certain width and height
WebView* view = web_core->CreateWebView(WIDTH, HEIGHT);
{% endhighlight %}

### Load a Page

Let's start loading a web-page into our view. It's a good time to mention that most API calls in Awesomium are asynchronous (they are not guaranteed to complete by the time the method returns).

{% highlight cpp %}
// Load a certain URL into our WebView instance
WebURL url(WSLit(URL));
view->LoadURL(url);
{% endhighlight %}

By the way, `WSLit()` is a special helper function that lets you declare __WebString literals__. Most of our API uses UTF-16 strings (wrapped with `WebString`) but we added `WSLit()` so you can declare ASCII C-strings with minimal fuss.

#### Wait Until the Page Has Finished Loading

Now we need to wait for the view to finish loading the page. The easiest way to do this is to loop until `WebView::IsLoading` returns false. It's important that you always call `WebCore::Update` within your update loop to give the framework a chance to dispatch events.

{% highlight cpp %}
// Wait for our WebView to finish loading
while (view->IsLoading())
  web_core->Update();
  
// Sleep a bit and update once more to give scripts and plugins
// on the page a chance to finish loading.
Sleep(300);
web_core->Update();
{% endhighlight %}

### Save the Rendered Page to a JPEG

Since our WebView is being rendered offscreen, we need to save our rendering surface to a JPEG so we can see the result.

You can access the surface of all offscreen WebViews by calling `WebView::surface()`. You will need to cast this surface to `BitmapSurface` (the default Surface type) to actually use it.

{% highlight cpp %}
// Get the WebView's rendering Surface. The default Surface is of
// type 'BitmapSurface', we must cast it before we can use it.
BitmapSurface* surface = (BitmapSurface*)view->surface();

// Make sure our surface is not NULL-- it may be NULL if the WebView 
// process has crashed.
if (surface != 0) {
  // Save our BitmapSurface to a JPEG image in the current
  // working directory.
  surface->SaveToJPEG(WSLit("./result.jpg"));
}
{% endhighlight %}

### Clean Up

Make sure to clean up everything that you create:

{% highlight cpp %}
view->Destroy();

WebCore::Shutdown();
{% endhighlight %}

### Further Reading

 * [Tutorial 2 - Displaying Your First Page](/tutorials/tutorial-2-displaying-your-first-page.html)
