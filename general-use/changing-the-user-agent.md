---
layout: page
title : Changing the User-Agent
group: General Use

---

We supply a default user-agent with 1.7 that is compatible with Chrome 18.

If you would like override the default user-agent, you can do so using WebConfig when initializing the WebCore, for example:

{% highlight cpp %}
#include <Awesomium/STLHelpers.h>

using namespace Awesomium;

int main () {
  WebConfig config;
  config.user_agent = WSLit("Mozilla/5.0 (iPhone; CPU iPhone OS 5_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B176 Safari/7534.48.3");
  
  WebCore* core = WebCore::Initialize(config);
  core->Update();
  core->Shutdown();
  
  return 0;
}
{% endhighlight %}