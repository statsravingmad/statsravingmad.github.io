---
title: The truncated Poisson
author: manos_parzakonis
layout: post
permalink: /the-truncated-poisson/
image:
  - 
aktt_notify_twitter:
  - no
categories:
  - statistics
tags:
  - code
  - counts
  - hessian
  - likelihood
  - poisson
  - R
  - truncated
---
A common model for counts data is the Poisson. There are cases however that we only record positive counts, ie there is a truncation of 0. This is the truncated Poisson model.

To study this model we only need the total counts and the sample size. This comes from the sufficient statistic principle as the likelihood is

<p style="text-align: center;">
  $$ logL=-n-frac{e^{-lambda } n}{1-e^{-lambda }}+frac{T}{lambda }$$,
</p>

<p style="text-align: left;">
  where $$ T=sum _X$$. Let&#8217;s set $$ T=160$$ and  $$ n=50$$.
</p>

<pre>sum.x=160
n=50
library(maxLik)

loglik &lt;- function(theta, n, sum.x) {
- n* log(exp(theta) - 1) + sum.x * log(theta)
}</pre>

The gradient is

<p style="text-align: center;">
  $$ frac{e^{-2 lambda } n}{left(1-e^{-lambda }right)^2}+frac{e^{-lambda } n}{1-e^{-lambda }}-frac{T}{lambda ^2}$$.
</p>

<!--more-->

<pre>gradlik &lt;- function(theta, n, sum.x){
 (n*exp(-2*theta))/(1-exp(-theta))^2
 +(n*exp(-theta))/(1-exp(-theta))-sum.x/(theta^2)
}</pre>

The second derivative (the hessian) is

<p style="text-align: center;">
  $$ -frac{2 e^{-2 lambda } n}{left(1-e^{-lambda }right)^2}-frac{e^{-lambda } n}{1-e^{-lambda }}-e^{-lambda } left(frac{2 e^{-2 lambda }}{left(1-e^{-lambda }right)^3}+frac{e^{-lambda }}{left(1-e^{-lambda }right)^2}right) n+frac{2 T}{lambda ^3}$$.
</p>

<pre>hesslik &lt;- function(theta, n, sum.x) {
 -((2*n*exp(-2*theta))/(1-exp(-theta))^2)
 -(n*exp(-theta))/(1-exp(-theta))
 -exp(-theta)*((2*exp(-2*theta))/(1-exp(-theta))^3
 +exp(-theta)/(1-exp(-theta))^2)*n+(2*sum.x)/theta^3}</pre>

Now, we simply call the s/t function of maxLik package to fit the model.

<pre>a &lt;- maxLik(loglik, start=3.2,method="Newton-Raphson",
n=n,sum.x=sum.x, print.level=2)
<span style="color: #339966;"># ----- Initial parameters: -----
# fcn value: 28.18494
#      parameter initial gradient free
# [1,]       3.2        -2.124718    1
# Condition number of the (active) hessian: 1
# -----Iteration 1 -----
# -----Iteration 2 -----
# -----Iteration 3 -----
# --------------
# gradient close to zero. May be a solution
# 3  iterations
# estimate: 3.048175
# Function value: 28.34853

</span></pre>

<span style="color: #000000;">There is an nice & handy function I found somewhere in the net (sorry I don&#8217;t remember the author;() to plot the likelihood of the truncated Poisson.<br /> </span>

<pre>plot.logL = function(Left, Right, Step){
 grid.x = seq(Left, Right, by = Step)
 plot(grid.x, loglik(grid.x, n, sum.x), type = "l", ,col="red",
 xlab = "theta", ylab = "logL",
 main = "Log likelihood for truncated Poisson")
}
plot.logL(2, 4, 0.00001);grid()</pre>

[<img class="aligncenter size-medium wp-image-944" title="tr.poiss" src="http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/02/tr-poiss.jpeg?resize=385%2C349" alt="" data-recalc-dims="1" />][1]

<span style="color: #339966;"> </span>  
<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->

 [1]: http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/02/tr-poiss.jpeg