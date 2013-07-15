---
layout: page
title : Loading Local Assets
group: General Use
weight: 20

---

### Loading Assets from the Filesystem

You can load pages and assets into a WebView from the local filesystem by using `WebView::LoadURL` with a local `file:///` URL.

{% highlight cpp %}
view->LoadURL(WebURL(WSLit("file:///C:/Users/awesomium/Documents/webpage.html")));
{% endhighlight %}

This is just like opening a local HTML file into your browser.

### Loading Assets from a DataSource

Instead of putting all of your assets in a local folder alongside your application, we recommend using a DataSource (or even a DataPakSource) to distribute sensitive assets. A DataSource gives you complete control over how the resource is loaded (allowing you to implement encrypted and/or compressed loading schemes).

**See [this article](using-data-sources.html) for more information on using DataSource.**

### Same-Origin Policy

By default, Awesomium uses Chrome's "same-origin" policy to prevent cross-site scripting requests. If you wish to disable this behavior please see `WebPreferences.enable_web_security`.