---
title: Google Search Partners Demystified (the GA way)
author: manos_parzakonis
layout: post
permalink: /google-search-partners-demystified/
aktt_notify_twitter:
  - no
  - no
categories:
  - measure
tags:
  - cpc
  - custom report
  - google advertising
  - google analytics
  - search partners
  - sem
---
<p style="text-align: justify;">
  One of the mysteries of Google Advertising has always been the fact that you only got to know aggregated info on the Google Search Partner that your ads were displayed. this could be only a tiny bit of nuisance but for certain accounts, with major spends and/or tight performance constrains this can be a problem. Now, normally when you can&#8217;t find something in AdWords you turn to Google Analytics to find the answer. This is the case here too. You can create an advanced filter and utilize the User-Defined value to store the original referrer.
</p>

> PAUSE : Your first thought would be to look under the referral sources and use as a secondary dimension the Ad Distribution Network, but google protects the referrer, so this is useless in our case.

<p style="text-align: justify;">
  Now, keeping all safety measures (<em>use a test profile</em> blah,blah,blah&#8230;) head to the Admin tab and select the profile that you wish to manipulate. <em>I apply the filter on the Paid Search profiles I&#8217;ve set up.</em>
</p>

***How to create the Filter***

<li style="text-align: justify;">
  Name the filter appropriately, I use the name <em>Search Partners</em>
</li>
<li style="text-align: justify;">
  Select Custom Filter > Advanced
</li>
<li style="text-align: justify;">
  In the Field A -> Extract A select Referral and enter <strong>(//)([^/]*)</strong>
</li>
<li style="text-align: justify;">
  Head to Output To -> Constructor and select User-Defined from the drop-down menu. Then fill in <strong>$A2</strong> in the blank space.
</li>
<li style="text-align: justify;">
  Leave everything else as is and you are ready!
</li>

[<img class="size-full wp-image-680 aligncenter" title="Google Analytics" src="http://i0.wp.com/parzakonis.info/wp-content/uploads/2012/05/Google-Analytics.png?resize=574%2C416" alt="" data-recalc-dims="1" />][1]

<p style="text-align: justify;">
  Now,  in a few hours you will have the following picture under the User-Defined view. You might observe that this is not that helpful. This is because the filter is applied to every referrer (this means organic, paid, affiliate etc traffic is included in the report).
</p>

<p style="text-align: center;">
  <a href="http://i0.wp.com/parzakonis.info/wp-content/uploads/2012/05/User-Defined-Google-Analytics.png"><img class="wp-image-691 aligncenter" title="User-Defined - Google Analytics" src="http://i0.wp.com/parzakonis.info/wp-content/uploads/2012/05/User-Defined-Google-Analytics.png?resize=693%2C337" alt="" data-recalc-dims="1" /></a>
</p>

The first workaround is to use the *paid search* segment. Others might have a paid search profile (or you should start one now!), in some cases one might have a hand a google/cpc profile. Any way you can gain from the filtering feature of the <a title="Custom Reports | GA" href="https://support.google.com/analytics/bin/answer.py?hl=en&answer=1033013&topic=1012046&rd=1" target="_blank">Custom Reports</a> that you can build in under 2 minutes! You can get my configured custom report <a title="Search Partners Custom Report | GA" href="http://goo.gl/c0SPg" target="_blank">here</a>. Hope you enjoy it <img src="http://i1.wp.com/www.statsravingmad.com/wp-includes/images/smilies/icon_wink.gif?w=768" alt=";)" class="wp-smiley" data-recalc-dims="1" />

<p style="text-align: center;">
  <a href="http://i2.wp.com/parzakonis.info/wp-content/uploads/2012/05/Google-Analytics-Custom-Report.png"><img class="wp-image-699 aligncenter" title="Google Analytics-Custom Report" src="http://i1.wp.com/parzakonis.info/wp-content/uploads/2012/05/Google-Analytics-Custom-Report-636x310.png?resize=636%2C310" alt="" data-recalc-dims="1" /></a>
</p>

 [1]: http://i0.wp.com/parzakonis.info/wp-content/uploads/2012/05/Google-Analytics.png