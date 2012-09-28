---
layout: page
title : Introduction to JavaScript Integration
group: JavaScript Integration
weight: 1

---

___This Section is Under Construction___

Using our API you can communicate with web-pages through JavaScript; pass data to/from the page, bind custom methods, create persistent global objects, and evaluate arbitrary scripts.

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

{% highlight js %}
function addChatMessage(nickname, message) {
  chat = document.getElementById('chat');
  chat.innerText = nickname + ": " + message;
}
{% endhighlight %}
	
You can call it directly from C++ like so:

{% highlight cpp %}
   // Retrieve the global 'window' object from the page
JSValue window = web_view->ExecuteJavascriptWithResult(
  WSLit("window"), WSLit(""));
  
if (window.IsObject()) {
  JSArray args;
  args.push_back("Bob");
  args.push_back("Hello world!");

  window.ToObject().Invoke(WSLit("addChatMessage"), args);
}
{% endhighlight %}
	
## Global JavaScript Objects

Sometimes you need objects to persist between pages (such as when exposing Application objects to a WebView). In such cases, you should use Global Javascript Objects, see the following article for more information: [Using Global JavaScript Objects](using-global-javascript-objects.html)

## Binding Custom Method Callbacks

Another nifty feature of remote Javascript objects is that you can bind C++ callbacks to them and invoke them from JavaScript. See the following article for more information: [Binding Custom JavaScript Methods](binding-custom-javascript-methods.html)