---
layout: page
title : What's New in 1.7.0
group: Changelogs
weight: 10

---
{% include JB/setup %}

## Changes in Awesomium 1.7.0 (Core)

### Major Core Changes
 * You can now handle SSL errors and display relevant UI to allow the user to ignore certain certificate errors. See `WebViewListener::Dialog::OnShowCertificateErrorDialog`.
 * You can now request SSL info for a certain web-page so that you can display relevant UI (for example, the green "lock" in Chrome for authenticated web-sites). See `WebView::RequestPageInfo`
 * Full page zoom has been added, see `WebView::ZoomIn` and `WebView::SetZoom `
 * You can now block certain page navigations (useful for custom blacklist/whitelist behavior). See `ResourceInterceptor::OnFilterNavigation`
 * You can now define properties on remote JSObjects asynchronously. This is useful for setting a large number of properties at once. See `JSObject::SetPropertyAsync`
 * You can now invoke methods on remote JSObjects asynchronously. See `JSObject::InvokeAsync`
 * You can now define a custom hostname for the remote Inspector to listen on (useful if you are behind a router). See `WebConfig::remote_debugging_host`

### Major API Changes

The following methods were added:

 * WebView::RequestPageInfo
 * WebView::DidOverrideCertificateError
 * WebView::routing_id
 * WebView::ZoomIn, ZoomOut, SetZoom, ResetZoom
 * WebView::set_sync_message_timeout
 * WebConfig::remote_debugging_host
 * WebViewListener::Dialog::OnShowPageInfoDialog
 * WebViewListener::Dialog::OnShowCertificateErrorDialog
 * WebViewListener::View::OnAddConsoleMessage
 * ResourceInterceptor:;OnFilterNavigation
 * ResourceInterceptor::OnWillDownload
 * JSObject::SetPropertyAsync
 * JSObject::InvokeAsync

### Bugfixes

The following issues were resolved:

 * Issue with WebSession::AddDataSource not matching uppercase names
 * Issue with WebView::DidCancelDownload
 * Issue with custom method names not being enumerated within JSObject::GetMethodNames
 * Issue with crash upon certain download events.

__See [this announcement](http://labs.awesomium.com/whats-new-in-1-7-0) for more info about this release.__
 
