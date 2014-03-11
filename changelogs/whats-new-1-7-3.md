---
layout: page
title : What's New in 1.7.3
group: Changelogs
weight: 2

---
{% include JB/setup %}

> These notes are for the core C++ library, <a href="http://wiki.awesomium.net/changelogs/whats-new-1-7-3.html">click here</a> for the .NET notes.


### Major Core Changes

 * Fixed bug within WebKit that caused GDI handles to constantly increase in main process. [(issue #4)](https://github.com/awesomium/awesomium-pub/issues/4#issuecomment-27012277)
 * Added API to allow users to execute custom JavaScript at the very beginning of every frame.

### Major API Changes

 * Added `WebConfig::user_script`
