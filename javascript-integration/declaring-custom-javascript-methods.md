---
layout: page
title : Declaring Custom JavaScript Methods
group: JavaScript Integration

---

Another nifty feature of remote JavaScript objects is that you can declare custom methods to be handled in C++ and invoke them from JavaScript. 

## Declaring Custom Methods

Here's an example:

{% highlight cpp %}
my_object->SetCustomMethod(WSLit("myCallback"), false);
{% endhighlight %}

The above code would add a new method, `myCallback`, to `MyObject` that, when called from any page via Javascript, will invoke `JSMethodHandler::OnMethodCall` with the object's remote ID, the name of the callback, and the arguments passed, if any.

For example, say you made a button that calls “myCallback” with a single string argument (”hello!”) when clicked:

{% highlight html %}
<input type="button" value="Click Me!"
       onclick="MyObject.myCallback('hello!')" />
{% endhighlight %}
		
You could then intercept this callback in `JSMethodHandler::OnMethodCall` (you’ll need to register your JSMethodHandler first via `WebView::set_js_method_handler`):

{% highlight cpp %}
void MyHandler::OnMethodCall(WebView* caller,
                             unsigned int remote_object_id, 
                             const WebString& method_name,
                             const JSArray& args) {
  if(remote_object_id == my_object_id &&
     method_name == WSLit("myCallback"))
       WebString value = args[0].toString(); // value is 'hello!'
}
{% endhighlight %}
	
### Declaring Methods with Return Values