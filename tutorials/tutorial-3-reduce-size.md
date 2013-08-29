---
layout: page
title : Tutorial 3 - Reduce the size of Awesomium
group: Tutorials

---

You can reduce the size of the Awesomium libraries by using **UPX** to compress the individual DLLs of the Awesomium SDK.

###Tutorial

* Download the latest version of UPX for your platform, here: [http://upx.sourceforge.net/](http://upx.sourceforge.net/)
* Copy the upx executable to the **_build/bin_** folder of the SDK.
* Open a Console/Terminal window and navigate to the **_build/bin_** folder of the SDK.
* Execute the following commands:

{% highlight bash %}
upx --best --lzma awesomium.dll
upx --best --lzma icudt.dll
{% endhighlight %}

You can now delete the copy of the upx executable from the **_build/bin_** folder of the SDK.

###Notes

* It is not necessary to reduce the size of any other libraries of the Awesomium SDK, since they are of relatively small size.
* Do not apply this technique to the managed assemblies or the Awesomium libraries accompanying **Awesomium.NET**; a packed version of Awesomium.NET will be available with the final release of version 1.7.
