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

### Create Global JavaScript Object

### Bind Custom Method

### Further Reading
