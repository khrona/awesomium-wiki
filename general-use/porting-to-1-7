---
layout: page
title : Porting to 1.7
group: General Use

---

The biggest API changes between 1.6 and 1.7 involve creating WebSessions, displaying the WebView Surface in your application, and integrating JavaScript with your application.

## More Control Over Session Data

You have the option of creating WebViews with the default, in-memory WebSession or creating your own independent sessions. This gives you finer-grained control over cookies, authentication, and any other user-data for each WebView. If you decide to do the latter, you'll need to create a WebSession and use it during the call to CreateWebView. 

WebSessions can be used by more than WebView. It is your responsibility to release the WebSession after all WebViews are done using it.

## You Can Now Provide Your Own "RenderBuffer" Implementation

We decided to allow users to provide their own "RenderBuffer" implementation (now called "Surface") so that they can handle paint/scroll calls directly instead of copying a buffer every frame.

To provide your own Surface implementation, you'll need to register a new SurfaceFactory with WebCore. Otherwise, if you liked the old way, the default surface is "BitmapSurface" which is very similar to the previous "RenderBuffer".

## LoadHTML and LoadFile Removed In Favor Of DataSources

We now allow you to provide your own resource loader via the DataSource API. You can access resources for a certain DataSource via the following URL syntax:

`asset://data_source_name/path/goes/here.html`

We recommend all users to use DataSources for local assets (instead of distributing raw HTML files on disk and loading them with LoadFile). You can still load raw HTML into a WebView by encoding it as a DataURI and passing it to LoadHTML but we recommend against this for performance reasons.

## JavaScript Integration Changes

Our entire JavaScript API has been rewritten so that users can manipulate V8 objects directly across process boundaries. Previously we only allowed "simple" JavaScript types and ignored any complex Objects. This prevented us from returning DOM elements or doing more exotic manipulation of objects on the page.

We introduced the concept of "Remote" and "Local" JS Objects in 1.7. 

Local objects only have properties (no custom methods) and are primarily used for declaring data to pass to methods. Local objects can be created using the JSObject constructor. (All JS Objects in 1.6 were Local)

Remote objects live within the V8 engine in a separate process and can have both Properties and Methods. These can only be returned by the WebView (they actually declared within the script context within the page).

See this article for more info: http://wiki.awesomium.com/javascript-integration/introduction-to-javascript-integration.html

## Other API Changes

There are some other API changes (most notably String types and WebPreferences) which you can learn more about in the following articles:

http://wiki.awesomium.com/getting-started/basic-concepts.html
http://wiki.awesomium.com/general-use/introduction-to-web-views.html
http://wiki.awesomium.com/javascript-integration/introduction-to-javascript-integration.html
