---
title: Exceptions tracking in Google Analytics
author: manos_parzakonis
layout: post
permalink: "/google-analytics-exceptions-tracking/"
excerpt: A note on tracking exceptions into your Google Analytics data without resulting into event tracking.
comments: true
categories:
- measure
tags:
- analytics
- exceptions
- errors
- google
published: false
---

<section id="table-of-contents" class="toc">
<header>
<h3>Overview</h3>
</header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

For long we were tracking exceptions in the websites using event tracking like this
~~~ javascript
<script>
window.onerror = function(message, url, line) {
    setTimeout(function(){
        // Initialize _gaq if it's not already initialized.
        // This way even if analytics is not already initialized,
        // we can keep track of the exceptions until it is.
        _gaq = window._gaq || [];

        _gaq.push([
            '_trackEvent',
            'JS Exception',
            message,
            document.location.href,
            0, // Value
            true // Non interactive
        ]);
    }, 10);

    return true;
};
~~~

However, the unification of app and desktop tracking brought some advances. Now, adding the following snippet in your Google Tag Manager is all it takes.

~~~ javascript
<script>
if (typeof window.onerror == "object")
{
  window.onerror = function (err, url, line)
  {
    ga('send', 'exception', {
      'exDescription': line + " " + err
      });
    };
  }
</script>
~~~

##### View the data
It is interesting that the web version of Google Analytics is not making front matter the recording of exceptions and there are no (standard) reports available. Then it is easy to create a custom report to see the data. Another use would be to add a widget in a dashboard.
