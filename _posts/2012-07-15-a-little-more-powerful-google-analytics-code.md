---
title: A (little more) Powerful Google Analytics Code
author: manos_parzakonis
layout: post
permalink: /a-little-more-powerful-google-analytics-code/
aktt_notify_twitter:
  - no
categories:
  - measure
tags:
  - code
  - google analytics
  - modification
  - tracking
  - web measurement
---
<p style="text-align: justify;">
  I presume you are using various tools to measure the impact of your posts and the performance of your website. It would be a safe guess that <a href="https://www.google.com/analytics/" target="_blank">Google Analytics</a> is the predicament tool (especially if using blogspots).
</p>

<p style="text-align: justify;">
  The standard code is a bit restraining however. In that case Cardinal Path has put together more tracking features in a modified Google Analytic Tracking Code (GATC). While there is no need to go into the details of the code itself by examining the basic script the advantages are obvious.
</p>

<pre lang="javascript"></pre>

<p style="text-align: justify;">
  As it&#8217;s obvious the GAS script enables outbound links&#8217; tracking, downloads, video interactions (YouTube and Vimeo) provided you are embedding them using iframes. Among other it use event tracking to let you know of the scrolling visitors use to navigate your websiteYou can get the details in the github repository of Google Analytics on Steroids (<a href="https://github.com/CardinalPath/gas" target="_blank">GAS</a>). Essentially you need to download the <a href="https://github.com/CardinalPath/gas/downloads" target="_blank">gas.js</a> file and upload it to your website (so you are the one hosting it in order to be safe) and then replace the GATC you already have in place with the above script.
</p>

<p style="text-align: justify;">
  You can ignore the
</p>

<pre lang="javascript">_gas.push(['_setDomainName', '.mydomain.com']);</pre>

<p style="text-align: justify;">
  if using one domain tracking.
</p>

<p style="text-align: justify;">
  Also make sure that if you are already implemented event tracking  for any of the categories listed above that you are keeping the same Category,Action,Label convention so you don&#8217;t break your statistics when you shift to this code. You can manipulate this via the opts parameter in every _qas.push
</p>

<pre lang="javascript">_gas.push(['_gasTrackDownloads', 'Downloads');</pre>

Now you are ready! Have fun <img src="http://i0.wp.com/www.statsravingmad.com/wp-includes/images/smilies/icon_smile.gif?w=768" alt=":)" class="wp-smiley" data-recalc-dims="1" />

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->