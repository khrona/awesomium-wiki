---
layout: page
title : Binding Custom JavaScript Methods
group: JavaScript Integration

---

Another nifty feature of remote Javascript objects is that you can bind C++ callbacks to them and invoke them from JavaScript. Here's an example:

	my_object->SetCustomMethod(WSLit("myCallback"), false);

The above code would add a new method, `myCallback`, to `MyObject` that, when called from any page via Javascript, will invoke `JSMethodHandler::OnMethodCall` with the object's remote ID, the name of the callback, and the arguments passed, if any.

For example, say you made a button that calls “myCallback” with a single string argument (”hello!”) when clicked:

	<input type="button" value="Click Me!"
		onclick="MyObject.myCallback('hello!')" />
		
You could then intercept this callback in `JSMethodHandler::OnMethodCall` (you’ll need to register your JSMethodHandler first via `WebView::set_js_method_handler`):

	void MyHandler::OnMethodCall(WebView* caller,
                                 unsigned int remote_object_id, 
                                 const WebString& method_name,
                                 const JSArray& args) {
		if(remote_object_id == my_object_id &&
		   method_name == WSLit("myCallback"))
			WebString value = args[0].toString(); // value is 'hello!'
	}