---
title: 'The distribution of rho&#8230;'
author: manos_parzakonis
layout: post
permalink: /the-distribution-of-rho/
aktt_notify_twitter:
  - no
categories:
  - statistics
tags:
  - code
  - distribution
  - hypothesis
  - p-value
  - pearson
  - R
  - test
---
There was a post [here][1] about obtaining non-standard p-values for testing the correlation coefficient. The R-library

<pre><a href="cran.r-project.org/package=SuppDists">SuppDists</a></pre>

deals with this problem efficiently.

<pre>library(SuppDists)

plot(function(x)dPearson(x,N=23,rho=0.7),-1,1,ylim=c(0,10),ylab="density")
plot(function(x)dPearson(x,N=23,rho=0),-1,1,add=TRUE,col="steelblue")
plot(function(x)dPearson(x,N=23,rho=-.2),-1,1,add=TRUE,col="green")
plot(function(x)dPearson(x,N=23,rho=.9),-1,1,add=TRUE,col="red");grid()

legend("topleft", col=c("black","steelblue","red","green"),lty=1,
		legend=c("rho=0.7","rho=0","rho=-.2","rho=.9"))&lt;/pre&gt;</pre>

This is how it looks like,  
[  
][2][<img class="aligncenter size-medium wp-image-1097" title="rho.dist" src="http://i2.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/03/rho-dist1.jpeg?resize=339%2C314" alt="" data-recalc-dims="1" />][3]  
Now, let&#8217;s construct a table of critical values for some arbitrary or not significance levels.

<pre>q=c(.025,.05,.075,.1,.15,.2)
xtabs(qPearson(p=q, N=23, rho = 0, lower.tail = FALSE, log.p = FALSE) ~ q )
<span style="color:#339966;"># q
#     0.025      0.05     0.075       0.1      0.15       0.2
# 0.4130710 0.3514298 0.3099236 0.2773518 0.2258566 0.1842217</span></pre>

We can calculate p-values as usual too&#8230;

<pre>1-pPearson(.41307,N=23,rho=0)
<span style="color:#339966;"># [1] 0.0250003</span></pre>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->

 [1]: http://talkstats.com/showthread.php?t=11308
 [2]: http://www.statsravingmad.com/blog/wp-content/uploads/2010/03/rho-dist.jpeg
 [3]: http://i2.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/03/rho-dist1.jpeg