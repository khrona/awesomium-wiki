---
layout: page
title : Introduction to JavaScript Integration
group: JavaScript Integration
weight: 1

---

___This Section is Under Construction___

Using our API you can communicate with web-pages through JavaScript: pass data to/from the page, bind custom methods, create persistent global objects, and evaluate arbitrary scripts.

## JSValue Class

The JSValue class is used as an intermediary to translate C++ types into Javascript variables and vice-versa. To get a better understanding of how this relationship works, here’s how types are mapped between Javascript and C++ using JSValue:

<table>
<tr><th>Javascript</th><th>C++</th></tr>
<tr><td>Number</td><td>double or int</td></tr>
<tr><td>Boolean</td><td>bool</td></tr>
<tr><td>String</td><td>WebString (UTF-16)</td></tr>
<tr><td>Object</td><td>JSObject</td></tr>
<tr><td>Array</td><td>JSArray</td></tr>
<tr><td>Null</td><td>JSValue::Null()</td></tr>
<tr><td>Undefined</td><td>JSValue::Undefined()</td></tr>
</table>


### JSValue Type: Strings

You can get the string representation of any JSValue via `JSValue::ToString`. It is safe to call this on a JSValue of any type.

### JSValue Type: Objects

Objects in Javascript are sort of like a "map" or "dictionary". Every key has a "string" type and every value can be any number of types.

As specified in the table above, these Objects are modeled in C++ with the JSObject class. 

There are two types of JSObjects, **Local** and **Remote**. 

#### Local JSObjects

Local objects only have properties (no custom methods) and are primarily used for declaring data to pass to methods. Local objects can be created using the JSObject constructor:


{% highlight cpp %}
JSObject my_object;
my_object.SetProperty(WSLit("name"), WSLit("Bob"));
my_object.SetProperty(WSLit("age"), 42)
{% endhighlight %}

#### Remote JSObjects

Remote objects live within the V8 engine in a separate process and can have both Properties and Methods.

For example, say you had an object ‘Person’ in JavaScript:

{% highlight js %}
var Person = {
   name: 'Bob',
   age: 22,
};
{% endhighlight %}
	
You could then interact with this object in C++:

{% highlight cpp %}
JSValue my_value = web_view->ExecuteJavascriptWithResult("Person");

if(my_value.IsObject()) {
  JSObject& person = my_value.ToObject();

  WebString name = person.GetProperty(WSLit("name")).ToString();
  // value of name is 'Bob'
     
  int age = person.GetProperty(WSLit("age")).ToInteger();
  // value of age is '22'
}
{% endhighlight %}

##### Lifetime of Remote Objects

All remote objects are reference-counted and collected by the V8 garbage collector. When you return an object to the main process via a JSObject proxy, this ref-count is incremented, and when the JSObject is destroyed, the ref-count is decremented.

You can think of remote JSObjects as a pointer to a V8 object on the page. Every time you make a copy of a JSObject (via its copy-constructor), only the reference (remote ID) is copied; a deep copy is not made.

When the V8 object goes away (usually due to a page navigation), the reference will no longer be valid and method calls may fail (see the section below). If you need an object to be persistent, you should create a Global JavaScript Object instead.

##### Handling Errors

Method calls on Remote objects are proxied to the child-process and execute synchronously but may fail for various reasons (see `JSObject::last_error()`).

{% highlight cpp %}
JSValue foobar = my_object.GetProperty(WSLit("foobar"));
if (my_object.last_error() != kError_None) {
  // handle error here (if any)
}
{% endhighlight %}

### JSValue Type: Arrays

On the C++ side, Arrays are modeled as a vector of JSValues (wrapped as JSArray).

So, for example, if you had the following array in Javascript:

{% highlight js %}
var myArray = [1, 2, "three", 4, "five"];
{% endhighlight %}
	
You could access it in C++ like so:

{% highlight cpp %}
JSValue my_value = web_view->ExecuteJavascriptWithResult("myArray");

if(my_value.IsArray()) {
   JSArray& myArray = myValue.ToArray();

   int first = myArray[0].ToInteger(); // value is '1'
   int second = myArray[1].ToInteger();  // value is '2'
   WebString third = myArray[2].ToString(); // value is 'three'
}
{% endhighlight %}

### Directly Calling JavaScript Functions

You can call a Javascript function directly via `JSObject::Invoke`.

Say you had the following function in Javascript:

	function addChatMessage(nickname, message) {
	   chat = document.getElementById('chat');
	   chat.innerText = nickname + ": " + message;
	}
	
You can call it directly from C++ like so:

    // Retrieve the global 'window' object from the page
	JSValue window = web_view->ExecuteJavascriptWithResult(
	  WSLit("window"), WSLit(""));
	  
	if (window.IsObject()) {
      JSArray args;
	  args.push_back("Bob");
	  args.push_back("Hello world!");
	
	  window.ToObject().Invoke(WSLit("addChatMessage"), args);
	}
	
## Global Javascript Objects

The best way to integrate your application with a web-page is by using Global Javascript Objects. These objects are linked to the lifetime of the WebView so this feature is quite useful for specifying persistent data for each WebView.

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

### Binding Custom Method Callbacks

Another nifty feature of these global Javascript objects is that you can bind C++ callbacks to them and invoke events from Javascript. For example:

	myWebView->SetCustomMethod(WSLit("myCallback"), false);

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