---
layout: page
title : What's New in 1.7.2
group: Changelogs
weight: 2

---
{% include JB/setup %}

> These notes are for the core C++ library, <a href="http://wiki.awesomium.net/changelogs/whats-new-1-7-2.html">click here</a> for the .NET notes.


### Major Core Changes

 * Added API to help mitigate unchecked memory usage during long run times.
 * Fixed bug with enumeration of global `JSObject` properties.
 * Fixed bug with logging behavior (output was not consistent and was occasionally flushed during runtime).
 * Fixed bug on Mac OSX so that `/Library/Frameworks` is searched by default when determining package path.

### Major API Changes

 * Added `WebSession::ClearCache`
 * Added `WebView::ReduceMemoryUsage`
 * Added `WebConfig::reduce_memory_usage_on_navigation`
 * Added `WebCore::Log`
