---
layout: page
title: Awesomium Wiki
tagline: Wiki for Awesomium, a Web UI Bridge for Native Apps
groups:
  - name: Getting Started
  - name: General Use
  
---
{% include JB/setup %}

{% for g in page.groups %}
## {{ g.name }}
<ul>
  {% assign pages_list = site.pages %}
  {% assign group = g.name %}
  {% include JB/pages_list %}
</ul>
{% endfor %}

## Highlighting Test

{% highlight cpp %}
	#include <Awesomium/WebCore.h>
	
	using namespace Awesomium;
	
	int main() {
	  WebCore* core = WebCore::Initialize(WebConfig());
	  core->CreateWebView(512, 512);
	  
	  core->Shutdown();
	  
	  return 0;
	}
{% endhighlight %}