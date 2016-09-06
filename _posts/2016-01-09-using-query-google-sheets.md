---
title: Google Analytics reporting using Query() in Google Sheets
author: manos_parzakonis
layout: post
permalink: "/google-analytics-reporting-using-query-function/"
excerpt: Query your google sheet data like a pro with QUERY().
comments: true
categories:
- measure
tags:
- analytics
- reporting
- API
- google apps
- google sheets
- sheets add-ons
- sql

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

###It's a revisit
We've shown how to create a dashboard in Google Sheets in a previous post for your Google Analytics data. In this post we will revisit the problem with a implementation twist, we will use the Google Analytics Add-on for Sheets and the `QUERY()` function.

###Our setup
We need to create two entities in our sheet, the data view and the transformation view. 
