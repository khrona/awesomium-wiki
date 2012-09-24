---
layout: page
title : Using Web-Sessions
group: General Use
weight: 1

---

### What is a WebSession?

A WebSession is responsible for storing all user-generated data (cookies, cache, authentication, etc.). It can either be purely in-memory or saved to disk (you will need to provide a writeable path to store the data at runtime).

Each WebView must have a WebSession to store its data into, you can associate multiple WebViews with each WebSession.

### The Default WebSession

The WebCore creates a default, in-memory WebSession upon initialization. This session is used for all WebViews unless otherwise specified.

### Creating a WebSession

You can create a WebSession like so:

{% highlight cpp %}
WebSession* my_session = web_core->CreateWebSession(
  WSLit("C:\\Session Data Path"), WebPreferences());
{% endhighlight %}

This session will synchronize all of its data to `C:\\Session Data Path\`.

If you would like to create an in-memory session, just pass an empty path for the first parameter.

#### Specifying Custom Preferences
You can supply custom preferences for each WebSession via the second parameter. Here's an example:

{% highlight cpp %}
WebPreferences prefs;
prefs.enable_plugins = false;
prefs.enable_smooth_scrolling = true;
prefs.user_stylesheet = WSLit("* { background-color: yellow !important; }");

// Create an in-memory session with our custom preferences
WebSession* my_session = web_core->CreateWebSession(WSLit(""), prefs);
{% endhighlight %}

### Destroying the WebSession
It is your responsibility to call WebSession::Release once you are done using the WebSession. If you forget to do this we can't guarantee that your WebSession will be saved to disk upon application exit.

{% highlight cpp %}
WebSession* my_session = web_core->CreateWebSession(
  WSLit(""), WebPreferences());
  
my_session->Release();
{% endhighlight %}

The session will not be destroyed until all WebViews associated with it have been destroyed.