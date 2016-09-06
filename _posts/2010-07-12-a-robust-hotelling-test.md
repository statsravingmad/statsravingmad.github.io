---
title: 'A robust Hotelling test&#8230;'
author: manos_parzakonis
layout: post
permalink: /a-robust-hotelling-test/
aktt_notify_twitter:
  - yes
  - yes
aktt_tweeted:
  - 1
categories:
  - statistics
tags:
  - bootstrap
  - code
  - hotelling
  - multivariate
  - R
  - robust
  - test
---
Recently I was in need of testing a mean vector. I wrote a few lines of code in R and had it done perfectly. Hotelling test is one of the least interesting test to me. never really figured out why&#8230;

At that time I had some time to search more about it. One of the most common things to search for a test is a robust version of it (at least that&#8217;s what I search for!). A little search in the 3rd page of google results leads to the following :

> ### **One-sample and two-sample robust Hotelling tests with fast and robust bootstrap**
> 
> The classical Hotelling test for testing if the mean equals a certain value or if two means are equal is modiﬁed into a robust one through substitution of the empirical estimates by the MM-estimates of location and scatter. The MM-estimator, using Tukey’s biweight function, is tuned by default to have a breakdown point of 50% and 95% location efﬁciency. This could be changed through the control argument if desired.
> 
> ### Robust Hotelling T2 test
> 
> Performs one and two sample Hotelling T2 tests as well as robust one-sample Hotelling T2 test.

The first uses MM and S estimators while the latter a Minimum Covariance Determinant one. You can get info on those on the links in the end of the post. What might be crucial to you is that MM/S estimators would be more time comsuming compared to MCD. A little demonstation is the following..<!--more-->

<pre>library(rrcov)</pre>

<pre>data(delivery)</pre>

<pre>delivery.x &lt;- delivery[,1:2]</pre>

<pre>T2.test(delivery.x)</pre>

<pre><span style="color: #339966;"># </span></pre>

<pre><span style="color: #339966;">#     One-sample Hotelling test</span></pre>

<pre><span style="color: #339966;"># </span></pre>

<pre><span style="color: #339966;"># data:  delivery.x </span></pre>

<pre><span style="color: #339966;"># T^2 = 21.0494, df1 = 2, df2 = 23, p-value = 6.365e-06</span></pre>

<pre><span style="color: #339966;"># alternative hypothesis: true mean vector is not equal to (0, 0)' </span></pre>

<pre><span style="color: #339966;">#  </span></pre>

<pre><span style="color: #339966;"># sample estimates:</span></pre>

<pre><span style="color: #339966;">#               n.prod distance</span></pre>

<pre><span style="color: #339966;"># mean x-vector   8.76   409.28</span></pre>

<pre>t0&lt;-Sys.time()</pre>

<pre>T2.test(delivery.x, method="mcd")</pre>

<pre><span style="color: #339966;"># </span></pre>

<pre><span style="color: #339966;">#     One-sample Hotelling test (Reweighted MCD Location)</span></pre>

<pre><span style="color: #339966;"># </span></pre>

<pre><span style="color: #339966;"># data:  delivery.x </span></pre>

<pre><span style="color: #339966;"># T^2 = 37.701, df1 = 2.000, df2 = 9.146, p-value = 3.829e-05</span></pre>

<pre><span style="color: #339966;"># alternative hypothesis: true mean vector is not equal to (0, 0)' </span></pre>

<pre><span style="color: #339966;">#  </span></pre>

<pre><span style="color: #339966;"># sample estimates:</span></pre>

<pre><span style="color: #339966;">#                n.prod distance</span></pre>

<pre><span style="color: #339966;"># MCD x-vector 6.190476 309.7143
</span>Sys.time()-t0
<span style="color: #339966;"># Time difference of 0.04200006 secs</span></pre>

<pre>library(FRB)</pre>

<pre>t0&lt;-Sys.time()
FRBhotellingMM(delivery.x)</pre>

<pre><span style="color: #339966;"># One sample Hotelling test based on multivariate MM-estimates
# (bdp = 0.5, eff = 0.95) </span></pre>

<pre><span style="color: #339966;"># data:  delivery.x </span></pre>

<pre><span style="color: #339966;"># T^2_R =  84.59 </span></pre>

<pre><span style="color: #339966;"># p-value =  0.0022 </span></pre>

<pre><span style="color: #339966;"># Alternative hypothesis : true mean vector is not equal to ( 0 0 ) </span>
Sys.time()-t0
<span style="color: #339966;"># Time difference of 4.859 secs
</span></pre>

Time consuming as it may is I would stick with the Bootstrap method. What would you do?

**Read more**

Roelant, E., Van Aelst, S., and Willems, G. (2008), &#8220;Fast Bootstrap for Robust Hotelling Tests,&#8221; COMPSTAT 2008: Proceedings in Computational Statistics (P. Brito, Ed.) Heidelberg: Physika-Verlag, to appear.

Willems G., Pison G., Rousseeuw P. and Van Aelst S. (2002), A robust hotelling test, *Metrika*, **55**, 125–138.

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->