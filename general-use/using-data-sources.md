---
layout: page
title : Using Data-Sources
group: General Use

---

### What is a DataSource?

DataSource is a special interface that you can sub-class to define your own resource loader.

### Defining Your Own DataSource

For example, let's create a special DataSource that simply returns one hard-coded HTML string:

{% highlight cpp %}
#include <Awesomium/DataSource.h>

using namespace Awesomium;

const char* html_str = "<h1>Hello World</h1>";

class MyDataSource : public DataSource {
 public:
  MyDataSource() { }
  virtual ~MyDataSource { }
  
  virtual void OnRequest(int request_id,
                         const Awesomium::ResourceRequest& request, 
                         const Awesomium::WebString& path) {
    if (path == WSLit("index.html"))
      SendResponse(request_id,
                   strlen(html_str),
                   html_str,
                   WSLit("text/html"));
  }
};
{% endhighlight %}

#### Asynchronous Request

You do not have to call `SendResponse` immediately within OnRequest; this event is non-blocking and you can call `SendResponse` at any point later from any other thread.

### Binding a DataSource to a WebSession

To actually use a DataSource for certain URLs, you will need to call `WebSession::AddDataSource`. Here's an example of use:

{% highlight cpp %}
DataSource* data_source = new MyDataSource();
web_session->AddDataSource(WSLit("MyApplication"), data_source);
{% endhighlight %}

This will bind your DataSource with all URLs that start with `asset://MyApplication/`. The `path` parameter of `DataSource::OnRequest` will contain whatever follows the last slash of this URL prefix.

You can now load the hard-coded page we defined earlier into a WebView via:

{% highlight cpp %}
view->LoadURL(WebURL(WSLit("asset://MyApplication/index.html")));
{% endhighlight %}

### Using DataPakSource

We realize that bundling resources with your application is a common problem so we created a simple DataPak format and a DataPak DataSource.

The general idea is that you create a DataPak from a folder of files (see `WriteDataPak`) before-hand and then use `DataPakSource` to load files from this format.

{% highlight cpp %}
#include <Awesomium/DataPak.h>

DataSource* data_source = new DataPakSource(WSLit("C:\\my_resources.pak"));
web_session->AddDataSource(WSLit("MyApplication"), data_source);
{% endhighlight %}
