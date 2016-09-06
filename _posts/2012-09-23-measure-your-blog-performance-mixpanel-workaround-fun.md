---
title: 'Measure your blog performance : Mixpanel workaround fun'
author: manos_parzakonis
layout: post
permalink: /measure-your-blog-performance-mixpanel-workaround-fun/
aktt_notify_twitter:
  - no
categories:
  - Digital Life
tags:
  - analytics
  - blog
  - events
  - mixpanel
  - pageviews
  - tracking
  - web
---
<p style="text-align: justify;">
  <em>This is mostly a measure blog so I can safely presume that if you are bloggers yourself then you are measuring the performance of your efforts using a web analytics solution. In case you don&#8217;t then you are missing some exciting stuff going on with your online ramblings I can assure you&#8230;</em>
</p>

Now, there is sufficient data to assume that you are using <a href="http://analytics.google.com" target="_blank">Google Analytics</a> (<a href="http://w3techs.com/technologies/details/ta-googleanalytics/all/all" target="_blank">Usage statistics and market share of Google Analytics for websites</a>) but there are so many tools out there to have a look out for. I use more than one tools in this blog (none of which messes with your privacy however!). One alternative story is <a href="http://mixpanel.com" target="_blank">Mixpanel</a>. I prefer Mixpanel when it comes to funnels plus you can define formulas making it really attracting. You can get some details from this Zippy Kid&#8217;s <a href="http://www.zippykid.com/2012/07/16/funnels-event-tracking-google-analytics-mixpanel/" target="_blank">post</a>. Now, how you can use it to your blog?

***Install the basic code***

As you may be familiar with, most analytics tools work with Javascript. Go to your project page and grab the code

<pre lang="javascript"></pre>

<p style="text-align: justify;">
  Now paste this into your footer (footer.php located in the Appearance > Editor in case of a WordPress blog) and you will have tracking in all your pages. Let&#8217;s not confuse however that the reason we implementing Mixpanel tracking into our blog, site or whatever isn&#8217;t pageviews but events with attributes that are of interest to us.
</p>

<p style="text-align: justify;">
  <em><strong>Code for special events</strong></em>
</p>

<p style="text-align: justify;">
  I really want to have a segmentation based on the posts that visitor view on my blog prior to reaching specific goals that I have defined. Say that one goal is someone to go to my CV page, when he goes there I feel so much richer that I credit myself with 1,5Eur (this buys me a Cola in Greece)
</p>

<pre lang="javascript"><!-- MixPanel Start !-->

<!-- MixPanel End --></pre>

<p lang="javascript">
  We just used the most basic function of Mixpanel,
</p>

<pre lang="javascript">mixpanel.track</pre>

which is a mix of trackEvent and trackPageview that <a href="http://analytics.google.com" target="_blank">Google Analytics</a> uses for his own tracking.  We can use it in plain to send a specific event to mixpanel but we can use it better in order to have more data at hand. So, say that I need to monitor the posts per Category and whether they supply code to readers. Then I can break this down to posts as well.

<pre lang="javascript"><!-- MixPanel Start !-->

<!-- MixPanel End --></pre>

Make sure you tag all your posts in a consistent way. You can get organised with a document where you keep all your tracking codes so your can simply paste them over your new blog theme when time comes :).

Another interesting point to track is whether visitors that use your search box get results or not. Paste the following inside the section of SearchResults.php that defines that results are delivered. You will identify it by something like the following that essentially marks a code area that has post (aka results for the corresponding search query)

<pre lang="php"><!--?php if ( have_posts() ) : ?--></pre>

Now insert your Mixpanel tag

<pre lang="javascript"><!-- MixPanel Start !-->

<!-- MixPanel End --></pre>

Of course you can assign a Value to this, in fact you should definetely do so&#8230;

Next go to the comments.php page and add a tag that fires an event when a comment is made and assign a value (I credit myself with 11Euros for each comment)

<pre lang="javascript"><!-- MixPanel Start !-->

<!-- MixPanel End --></pre>

So, till now we managed to set Goals,Posts and Comments events assign value to each event, pass category and some Boolean data to Mixpanel. If I knew the first thing about php and Javascript I would definitely go the extra step and pass via WordPress functions the category & post title to automate further the script instead of hard coding the tracking code for each post.  
<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->