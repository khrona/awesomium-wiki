---
layout: page
title : Sharing Render Processes Between Web-Views
group: General Use

---

<p class="intro">We'll show you how to reduce memory and active processes by sharing renderers between Web-Views</p>

Each WebView that you spawn via `WebCore.CreateWebView` gets its own render process (eg, `awesomium_process.exe`) with its own set of resources. This is great for isolation and multi-process rendering but sometimes you need to create boatloads of views and don't want to spend boatloads of resources.

### Child Process Trick

One way to force WebViews to share a render process is to make one master WebView and use it to spawn a bunch of smaller web views (e.g., `window.open()` in JavaScript). Child WebViews will share the same process as the parent.
