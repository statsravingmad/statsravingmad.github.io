---
title: A tiny RCurl headache ;)
author: manos_parzakonis
layout: post
permalink: /a-tiny-rcurl-headache/
aktt_notify_twitter:
  - no
categories:
  - statistics
tags:
  - data
  - google
  - GoogleDocs
  - R
  - spreadsheets
  - web
---
<p style="text-align: justify;">
  As more and more data go online (plus we love <a href="https://drive.google.com/start" target="_blank">Google Drive</a>) we are forced to connect to our data over the net. We mostly do this via <a href="http://cran.r-project.org/web/packages/RCurl/index.html" target="_blank">RCurl</a> (but we could do this using <a href="https://github.com/duncantl/RGoogleDocs" target="_blank">RGoogleDocs</a> as well).In that case all that is required to get the data into R is the two lines of code
</p>

<pre lang="rsplus">require(RCurl)
myCsv&lt;-getURL("https://docs.google.com/spreadsheet/pub?key=0AiD-
XfJ7BS9udG90TG9RZUQ2ZFZXMTRRNVZYU0Uxb1E&#038;output=csv")</pre>

<p style="text-align: justify;">
  However, it's possible you end up with a message like this...All in all the point is that the data load *failed*.
</p>

<pre lang="rsplus">Error in function (type, msg, asError = TRUE)  : 
  SSL certificate problem, verify that the CA cert is OK. Details:
error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed</pre>

<p style="text-align: justify;">
  In that case the solution is to add the <strong>ssl.verifypeer = FALSE</strong> to the getURL function to override the default behavior of R.
</p>

<pre lang="rsplus">myCsv&lt;-getURL("https://docs.google.com/spreadsheet/pub?key=0AiD-
XfJ7BS9udG90TG9RZUQ2ZFZXMTRRNVZYU0Uxb1E&#038;output=csv", ssl.verifypeer = FALSE)</pre>

<p style="text-align: justify;">
  <em><strong>Note:</strong></em> The above holds for <a href="https://github.com/duncantl/RGoogleDocs" target="_blank">RGoogleDocs</a> too, given that the package depends on <a href="http://cran.r-project.org/web/packages/RCurl/index.html" target="_blank">RCurl</a> for the heavy lifting.
</p>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->