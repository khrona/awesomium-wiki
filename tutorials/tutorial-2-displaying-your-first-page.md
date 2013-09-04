---
layout: page
title : Tutorial 2 - Displaying Your First Page
group: Tutorials
weight: 2

---

> This tutorial is under construction, check back soon.

In this tutorial, we'll load our own HTML into a WebView and display it within a window.

### Start With This Code

It's quite a bit of work to set up an empty Windows GDI or Mac OSX Cocoa application. To save some time we've gone ahead and done some of the work for you.

Start by downloading one of the following frameworks and following the README for your platform:

 * [Tutorial Framework for Awesomium (GitHub ZIP)](https://github.com/awesomium/tutorial-framework/archive/master.zip)

#### Windowed WebViews

It's important to note that we will be using Windowed WebViews (as opposed to Offscreen WebViews) with the Tutorial Framework. These views render directly to a platform-specific graphics surface and capture input as well.

### Loading Custom HTML

After setting up your project using the tutorial framework, open up `main.cc` and find the following code:

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(500, 300);
     // < Set up your View here. >
  }
{% endhighlight %}

The `OnLoaded` method is called within the tutorial framework once the application has finished launching. This is your best chance to set up your WebViews and make them do interesting things.

The framework uses a special wrapper class called `View` which consists of a platform window and a WebView. Notice the code above creates one View with dimensions of 512x512.

If you ran this code right now, nothing would be displayed because the WebView contains no content. Let's fix that.

Replace the `// <Set up your View here. >` code with the following:

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(500, 300);

    WebURL url(WSLit("data:text/html,<h1>Hello World</h1>"));
    view_->web_view()->LoadURL(url);
  }
{% endhighlight %}

If you build and run this code, you should get something that looks likes this:

![Screenshot 1](/assets/images/tutorial-2/screen-1.png)

#### Data URIs

You'll notice that we used a special type of URL called a "Data URI" to load our custom HTML into our WebView. This is a quick and easy way to pass strings of HTML but it definitely is not the best way.

#### Local HTML Files

A better way might be to store our HTML content in a local file with our application.

Create a new HTML file somewhere on your hard drive and add the following content to it 

{% highlight html %}
<html>
<body>
<h1>Hello World</h1>
<script type="text/javascript">
document.write("You are running Awesomium " + awesomium.version);
</script>
</body>
</html>
{% endhighlight %}

Now let's modify our code so that it loads our local HTML file instead, you'll need to edit the path so that it points to your file:

{% highlight cpp %}
  // Inherited from Application::Listener
  virtual void OnLoaded() {
    view_ = View::Create(500, 300);

    WebURL url(WSLit("file:///C:/Users/awesomium/Documents/app.html"));
    view_->web_view()->LoadURL(url);
  }
{% endhighlight %}

If everything went as expected, you should get something similar to the following:

![Screenshot 2](/assets/images/tutorial-2/screen-2.png)

#### Custom Data Sources

Loading local files is a great solution if you don't mind distributing all of your interface assets in one of your application's folders.

If you need a little more security or already have your own resource loader, we recommend taking a look at our [DataSource API](/general-use/using-data-sources.html) for a better approach.

### Further Reading

