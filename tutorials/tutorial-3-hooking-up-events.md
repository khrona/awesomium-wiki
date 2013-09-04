---
layout: page
title : Tutorial 3 - Hooking Up Events
group: Tutorials
weight: 3

---

<p class="intro">In this tutorial, we'll show how to hook up events between the page and host application via JavaScript callbacks.</p>

### Start With This Code

It's quite a bit of work to set up an empty Windows GDI or Mac OSX Cocoa application. To save some time we've gone ahead and done some of the work for you.

Start by downloading the framework below and following the README for your platform:

 * [Tutorial Framework for Awesomium (GitHub ZIP)](https://github.com/awesomium/tutorial-framework/archive/master.zip)
 
### Create Your HTML Interface

Create a new HTML file named `app.html` somewhere on your hard drive and add the following content to it:

{% highlight html %}
<html>
<body>
<h1>Welcome to My App</h1>
<button onclick="app.sayHello()">Say Hello</button>
</body>
</html>
{% endhighlight %}

### Load The File

After setting up your project using the tutorial framework, open up `main.cc` and find the following code:

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(500, 300);
     // < Set up your View here. >
  }
{% endhighlight %}

Replace the `// <Set up your View here. >` code with the following, you'll need to <span class="highlight">adjust the file path</span> so that it points to your own `app.html`:

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(500, 300);
    
    WebView* web_view = view_->web_view();

    WebURL url(WSLit("file:///C:/Users/awesomium/Documents/app.html"));
    web_view->LoadURL(url);
  }
{% endhighlight %}

Great, our app should now display the HTML file we created a little earlier. If you build and run now, you should get something that looks like this:

![Screenshot 1](/assets/images/tutorial-3/screen-1.png)

You'll notice if you click the button right now, it won't do anything. We need to modify our application so that it responds to calls from `app.sayHello()` via JavaScript.

### Create Global JavaScript Object

Let's create a special JavaScript object that will stay alive throughout the entire lifetime of our WebView. This will allow us to expose certain methods and properties that our pages can use via JavaScript.

Add the following code underneath your LoadURL code:

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(500, 300);
    
    WebView* web_view = view_->web_view();

    WebURL url(WSLit("file:///C:/Users/awesomium/Documents/app.html"));
    web_view->LoadURL(url);
    
    JSValue result = web_view->CreateGlobalJavascriptObject(WSLit("app"));
  }
{% endhighlight %}

### Binding Custom Methods

We now need to declare a custom method named `sayHello` to JavaScript and then bind this method to a callback in C++.

To do this, we're going to save some time and use a special helper class included in the tutorial framework called `MethodDispatcher`.

#### Include MethodDispatcher

At the top of your application, add `#include 'method_dispatcher.h` underneath `#include 'view.h`:

{% highlight cpp %}
#include "application.h"
#include "view.h"
#include "method_dispatcher.h"
#include <Awesomium/WebCore.h>
#include <Awesomium/STLHelpers.h>
#ifdef _WIN32
#include <Windows.h>
#endif

using namespace Awesomium;
{% endhighlight %}

#### Define Our Callback

#### Bind Our Method

### Further Reading
