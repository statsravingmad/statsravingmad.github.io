---
title: 'A von Mises variate&#8230;'
author: manos_parzakonis
layout: post
permalink: /a-von-mises-variate/
image:
  - 
aktt_notify_twitter:
  - no
categories:
  - probability
  - statistics
tags:
  - algorithm
  - code
  - distribution
  - metropolis
  - R
  - simulation
  - von mises
---
Inspired from a mail that came along the [previous][1] random generation post the following question rised :

> How to draw random variates from the Von Mises distribution?

First of all let&#8217;s check the pdf of the probability rule, it is $$ f(x):=frac{e^{b text{Cos}[y-a]}}{2 pi text{BesselI}[0,b]}$$, for $$-pi leq xleq pi $$.

Ok, I admit that Bessels functions can be a bit frightening, but there is a work around we can do. The solution is a Metropolis algorithm simulation. It is not necessary to know the normalizing constant, because it will cancel in the computation of the ratio. The following code is adapted from [James Gentle&#8217;s][2] notes on [Mathematical Statistics][3] .<pre lang="rsplus" line=”1″ escaped="true"> n <- 1000 x <- rep(NA,n) a <-1 c <-3 yi <-3 j <-0 i<-2 while (i < n) { i<-i+1 yip1 <- yi + 2\*a\*runif(1)- 1 if (yip1 < pi & yip1 > - pi) { if (exp(c*(cos(yip1)-cos(yi))) > runif(1)) yi <- yip1 else yi <- x[i-1] x[i] <- yip1 } } hist(x,probability=TRUE,fg = gray(0.7), bty="7") lines(density(x,na.rm=TRUE),col="red",lwd=2)</pre> 

[<img class="aligncenter size-medium wp-image-1142" title="vm.rg" src="http://i2.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/03/vm-rg.jpeg?resize=342%2C342" alt="" data-recalc-dims="1" />][4]

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->

 [1]: http://statsravingmad.wordpress.com/2010/03/16/in-search-of-a-random-gamma-variate/
 [2]: http://mason.gmu.edu/~jgentle/
 [3]: http://mason.gmu.edu/~jgentle/csi9723/MathStat.pdf
 [4]: http://i2.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/03/vm-rg.jpeg