---
layout: page
title : What's New in 1.7.2
group: Changelogs
weight: 1

---
{% include JB/setup %}


### Major Core Changes

 * Added API to help mitigate unchecked memory usage during long run times.
 * Fixed bug with enumeration of global JS Object properties.
 * Fixed bug with logging behavior (undefined output).
 * Fixed bug on Mac OSX so that /Library/Frameworks is searched by default when determining package path.

### Major API Changes

 * Added WebSession::ClearCache
 * Added WebView::ReduceMemoryUsage
 * Added WebConfig::reduce_memory_usage_on_navigation
 * Added WebCore::Log
