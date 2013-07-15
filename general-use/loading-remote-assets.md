---
layout: page
title : Loading Remote Assets
group: General Use
weight: 21

---

### Remote HTML Assets

You can load any HTTP URL into a WebView just as you would in a regular browser by using `WebView::LoadURL`.

For example:

{% highlight cpp %}
view->LoadURL(WebURL(WSLit("http://www.google.com")));
{% endhighlight %}

Please note that you can only load HTML and text files into a WebView at this time.

### Same-Origin Policy

By default, Awesomium uses Chrome's "same-origin" policy to prevent cross-site scripting requests. If you wish to disable this behavior please see `WebPreferences.enable_web_security`.