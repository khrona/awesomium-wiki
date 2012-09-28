---
layout: page
title : Using Global JavaScript Objects
group: JavaScript Integration

---

Sometimes it's useful to store persistent state between pages. This is where Global JavaScript Objects come in-- these objects are linked to the lifetime of the WebView and never change their remote ID.

All global objects are 'remote' (their state is stored in the WebView's child-process).

### Creating Global JS Objects

Creating, manipulating, and accessing these objects is very simple in our C++ API. To create an object, simply call `WebView::CreateGlobalJavaScriptObject` with the name that you want the object to appear as in Javascript.

For example:

	JSValue result = web_view->CreateGlobalJavascriptObject(
	  WSLit("MyObject"));
	
The above would create a global Javascript object that you can access as `MyObject` from any web page loaded into your WebView.

You can coerce the result into an actual JSObject instance like so:

    JSObject& my_object = result.ToObject();
    
### Setting Properties

Of course, creating objects alone isn’t very useful–- to give the object some properties, you can use `JSObject::SetProperty`:

	my_object->SetProperty(WSLit("name"), WSLit("foobar"));
	my_object->SetProperty(WSLit("color"), WSLit("Blue"));
	my_object->SetProperty(WSLit("level"), 25);
	
You can set as many properties as you want, setting a property more than once will replace the previous value. Notice that the last parameter to `SetProperty` accepts a JSValue (more on that later).
