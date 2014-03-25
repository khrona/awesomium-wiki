---
layout: page
title : What's New in 1.7.4
group: Changelogs
weight: 1

---
{% include JB/setup %}

> These notes are for the core C++ API, __<a href="http://wiki.awesomium.net/changelogs/whats-new-1-7-4.html">click here</a> for the .NET notes__.


### Major Core Changes
 * Removed quota limit on local storage and session storage 
 * Fixed issue with `WebConfig::user_script` on Mac OSX
 * Fixed issue with `about:blank` being added to history at beginning of `WebView` lifetime
 * Fixed issue with PDF files not being downloaded when Adobe Reader is installed 
 * Fixed crash that occurs when `JSObject::Invoke` fails on executing a callback with invalid JavaScript
 * Fixed crash issue when JavaScript is disabled
 * Fixed crash issue with `requestQuota`
 * Fixed crash when users hit `CTRL+LEFT` at beginning of line

### Major API Changes
 * Added `WebConfig::asset_protocol`
 * Added catch-all rule to `WebSession::AddDataSource`
 * Added host and request parameters to `DataSource::OnRequest`
