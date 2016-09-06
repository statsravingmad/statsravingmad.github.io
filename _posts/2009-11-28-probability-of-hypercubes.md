---
title: 'Probability of hypercubes&#8230;'
author: manos_parzakonis
layout: post
permalink: /probability-of-hypercubes/
categories:
  - statistics
tags:
  - calculation
  - function
  - multivariate
  - normal
  - package
  - probabilities
  - R
---
&#8230;in R of course!

There is a handy function to do those calculations. Normally (ahh!) you might resolve to a symbolic calculation package (Maple,Mathematica etc.)  but that is not the situation any more. The calculations are done with the [mnormt][1] package. Relevant functions exist in other packages as well ([R : Distributions][2])

> <pre>x &lt;- seq(-2,4,length=21)
y &lt;- 2*x+10
z &lt;- x+cos(y)
mu &lt;- c(1,12,2)
Sigma &lt;- matrix(c(1,2,0,2,5,0.5,0,0.5,3), 3, 3)
p2 &lt;- sadmvn(lower=rep(-Inf,2), upper=c(2, 11), mu[1:2], Sigma[1:2,1:2])
&gt; p2
[1] 0.3273202
attr(,"error")
[1] 2e-16
attr(,"status")
[1] "normal completion"</pre>

 [1]: http://cran.r-project.org/web/packages/mnormt/index.html
 [2]: http://cran.r-project.org/web/views/Distributions.html