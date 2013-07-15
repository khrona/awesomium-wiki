---
layout: page
title : Handling Child Web-Views
group: General Use

---

### Child Web-Views

Pages loaded into a WebView may occasionally need to create a new 'window' to display external links (for example, links with `target="_blank"` or calls to `window.open()` in JavaScript). To handle this, each WebView can create a new child WebView that automatically loads content.

#### Displaying the View

It is your responsibility to display these child WebViews within your application in some way that makes sense. This usually means making a subclass of `WebViewListener::View`, registering it with your WebView, and handling the `OnShowCreatedWebView` method.

Here's an example of how the WebFlow sample handles this method:

{% highlight cpp %}
void Application::OnShowCreatedWebView(Awesomium::WebView* caller,
                                       Awesomium::WebView* new_view,
                                       const Awesomium::WebURL& opener_url,
                                       const Awesomium::WebURL& target_url,
                                       const Awesomium::Rect& initial_pos,
                                       bool is_popup) {
  // Resize the new WebView immediately to our container's dimensions
  new_view->Resize(WIDTH, HEIGHT);
  
  // Special overload of the WebTile container class that displays
  // an existing WebView on an OpenGL surface.
  WebTile* new_tile = new WebTile(new_view, WIDTH, HEIGHT);
  
  new_view->set_view_listener(this);
  new_view->set_load_listener(this);
  new_view->set_process_listener(this);

  webTiles.push_back(new_tile);
  animateTo(webTiles.size() - 1);
}
{% endhighlight %}

#### Windowed Child Web-Views

All windowed Web-Views will create child WebViews that are also windowed. You should make sure to immediately call `WebView::set_parent_window` on the new child WebView within `OnShowCreatedWebView`.

