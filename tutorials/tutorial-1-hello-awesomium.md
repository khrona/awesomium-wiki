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
#include <iostream>
#if defined(__WIN32__) || defined(_WIN32)
#include <windows.h>
#elif defined(__APPLE__)
#include <unistd.h>
#endif

// Various macro definitions
#define WIDTH   800
#define HEIGHT  600
#define URL     "http://www.google.com"

using namespace Awesomium;

// Forward declaration of our update function
void Update(int sleep_ms);

// Our main program
int main() {

  // Your program goes here.
  
  return 0;
}

void Update(int sleep_ms) {
  // Sleep a specified amount
#if defined(__WIN32__) || defined(_WIN32)
  Sleep(sleep_ms);
#elif defined(__APPLE__)
  usleep(sleep_ms * 1000);
#endif

  // Put your Update code here.
}
{% endhighlight %}


#### Initialize the Framework

#### Create the WebView

#### Load a Page

##### Wait Until the Page Has Finished Loading

#### Save the Rendered Page to a JPEG

#### Clean Up

### Further Reading