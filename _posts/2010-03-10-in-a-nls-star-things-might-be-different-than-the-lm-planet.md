---
title: 'In a nls star things might be different than the lm planet&#8230;'
author: manos_parzakonis
layout: post
permalink: /in-a-nls-star-things-might-be-different-than-the-lm-planet/
aktt_notify_twitter:
  - no
categories:
  - statistics
tags:
  - lm
  - nls
  - nonlinear
  - R
  - regression
---
The nls() function has a well documented (and [discussed][1]) different behavior compared to the lm()&#8217;s. Specifically you can&#8217;t just put an indexed column from a data frame as an input or output of the model.

<pre>&gt; nls(data[,2] ~ c + expFct(data[,4],beta), data = time.data,
+ start = start.list)
<span style="color:#339966;">Error in parse(text = x) : unexpected end of input in "~ "
</span></pre>

The following will work, when we assign things as vectors.

<pre>&gt; nls(y ~ c + expFct(x,beta), data = time.data,start = start.list)
<span style="color:#339966;">#
# Formula: y ~ c + expFct(x,beta)
#
# Parameters:
#        Estimate Std. Error t value Pr(&gt;|t|)    
# c     3.7850419  0.0042017  900.83  &lt; 2e-16 ***
# beta  0.0053321  0.0003733   14.28 1.31e-12 ***
# ---
# Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#
# Residual standard error: 0.01463 on 22 degrees of freedom
#
# Number of iterations to convergence: 1
# Achieved convergence tolerance: 7.415e-06</span></pre>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->

 [1]: https://stat.ethz.ch/pipermail/r-help/2008-June/thread.html#165981