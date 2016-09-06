---
title: Track the leads to your 404 pages
author: manos_parzakonis
layout: post
permalink: /track-the-leads-to-your-404-pages/
aktt_notify_twitter:
  - no
fb_social_plugin_settings_box_like:
  - default
fb_social_plugin_settings_box_subscribe:
  - default
fb_social_plugin_settings_box_send:
  - default
fb_social_plugin_settings_box_comments:
  - default
fb_social_plugin_settings_box_recommendations_bar:
  - default
categories:
  - Digital Life
  - measure
tags:
  - 404
  - error
  - event
  - google analytics
  - tracking
---
<p style="text-align: justify;">
  A common issue when having a lot of interlinking in your content is that at some point of time you will either delete or mistype a link URL (you soooo rush to push the Publish button). There are two ways ot track the page that originated to the 404 mishappening&#8230;The first one is to use the Navigation Summary, the second one is to use <a href="http://doteduguru.com/id7229-idiots-guide-to-event-tracking.html" target="_blank">Event Tracking</a>. If you are not familiar with event tracking it is a google analytics gem that you can use to measure interactions with elements of your site and many more. To implement Event tracking you will need to add the following code snippet in the 404 page of your site to track the previous page that originated to the 404 mishappening&#8230;
</p>

<pre lang="Javascript"></pre>

<p style="text-align: justify;">
  Then you will see in your Event Reports the Event Category 404.
</p>

[<img class="aligncenter size-large wp-image-2255" title="404_event_category" src="http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2012/10/404_event_category-1024x126.jpg?resize=768%2C95" alt="" data-recalc-dims="1" />][1]

<p style="text-align: justify;">
  Drilling down to the Event Action will reveal the page <strong>before</strong> that lead to the 404 page. This way you can spot the page and possibly the broken link (most 404 pages stem from broken links or user typing a wrong URL)
</p>

[<img class="aligncenter size-large wp-image-2270" title="404 Page Lead" src="http://i1.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2012/10/Top-Events-Google-Analytics-1024x43.png?resize=768%2C32" alt="" data-recalc-dims="1" />][2]

Now I should go and fix the broken link in the Software page of mine : )

NOTE : If you are using WordPress you will find the previous page appended to the 404 URL, like this, where the homepage is generating the error

<pre lang="html">/404.html?page=/blog/</pre>

 [1]: http://i2.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2012/10/404_event_category.jpg
 [2]: http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2012/10/Top-Events-Google-Analytics.png
