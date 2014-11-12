---
layout: page
title : What's New in 1.7.5
group: Changelogs
weight: 5

---
{% include JB/setup %}

> These notes are for the core C++ API, __<a href="http://wiki.awesomium.net/changelogs/whats-new-1-7-5.html">click here</a> for the .NET notes__.

### Features / API Changes
 * Added ResourceRequest::set_ignore_data_source_handler
 * Added WebPreferences::max_http_cache_storage
 * Added WebPreferences::user_script
 * Added WebConfig::user_stylesheet
 * Made it so user_script and user_stylesheet values in WebConfig and WebPreferences are concatenated
 * Added WebViewListener::Process::OnLaunch
 * Made DataSource::SendResponse buffer param const
 
### Bugfixes
 * Fixed crash on Windows XP when using Facebook Connect (and other sites with similar certificate signing modes)
 * Fixed crash on Linux when rendering unstyled checkboxes, buttons, and progress bars. _Note: to actually display unstyled checkboxes, radio buttons, or
progress bars, users will need to declare global CSS so that these widgets are painted by WebKit instead._
 * Fixed crash that occurs if user unfocuses a textbox during an IME composition
 * Fixed crash with very large strings of `user_script`

### .NET Changes
 * <a href="http://wiki.awesomium.net/changelogs/whats-new-1-7-5.html">Click here</a> for the .NET notes
