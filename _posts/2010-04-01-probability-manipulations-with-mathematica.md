---
title: 'Probability manipulations with Mathematica&#8230;'
author: manos_parzakonis
layout: post
permalink: /probability-manipulations-with-mathematica/
image:
  - 
aktt_notify_twitter:
  - no
categories:
  - probability
  - statistics
tags:
  - characteristic function
  - estimation
  - Mathematica
  - moment generating function
---
There comes a time that a statistician needs to do some ananytic calculations. There more than a bunch of tools to use but I usually prefer Mathematica or Maple. Today, I&#8217;m gonna use Mathematica to do a simple exhibition.

Let&#8217;s set this example upon the  $$ U(2 theta \_1-theta \_2leq xleq 2 theta \_1+theta \_2) $$  distribution.

<pre>pfun = PDF[UniformDistribution[{2*Subscript[θ, 1] - Subscript[θ, 2],
2*Subscript[θ, 1] + Subscript[θ, 2]}], x]</pre>

<p style="text-align:center;">
  $$ begin{cases}<br /> frac{1}{2 theta _2} & 2 theta _1-theta _2leq xleq 2 theta _1+theta _2 \<br /> 0 & text{True}<br /> end{cases} $$
</p>

<p style="text-align:left;">
  One of the most intensive calculations is the characteristic function (eq. the moment generating function). This is straightforward to derive.
</p>

<pre>cfun=CharacteristicFunction[UniformDistribution[
{2*Subscript[θ, 1]-Subscript[θ, 2],2*Subscript[θ, 1]+Subscript[θ, 2]}],x]</pre>

<p style="text-align:center;">
  $$ -frac{i left(-e^{i x left(2 theta _1-theta _2right)}+e^{i x left(2 theta _1+theta _2right)}right)}{2 x theta _2} $$.
</p>

The Table[] command calculates for us the raw moments for our distribution.

<pre>Table[Limit[D[cfun, {x, n}], x -&gt; 0]/I^n, {n, 4}]</pre>

<p style="text-align:center;">
  $$ left{2 theta _1,frac{1}{3} left(12 theta _1^2+theta _2^2right),2 theta _1 left(4 theta _1^2+theta _2^2right),16 theta _1^4+8 theta _1^2 theta _2^2+frac{theta _2^4}{5}right} $$.
</p>

Calculate the sample statistics.

<pre>T=List[8.23,6.9,1.05,4.8,2.03,6.95];
{Mean[T],Variance[T]}</pre>

<p style="text-align:center;">
  $$ {4.99333,8.46171} $$.
</p>

<p style="text-align:left;">
  Now, we can use a simple moment matching technique to get estimates for the parameters.
</p>

<pre>Solve[{Mean[T]-2*Subscript[θ, 1]==0,-(2*Subscript[θ, 1])^2+
1/3 (12 Subscript[θ, 1]^2+!*SubsuperscriptBox[(θ), (2), (2)])-
Variance[T]==0},{Subscript[θ, 2],Subscript[θ, 1]}]</pre>

<p style="text-align:center;">
  $$left{left{theta _1to 2.49667,theta _2to -5.03836right},left{theta _1to 2.49667,theta _2to 5.03836right}right} $$.
</p>

<p style="text-align:left;">
  Check the true value for the $$ theta _2$$.
</p>

<pre>Reduce[2 Subscript[θ, 1]-Subscript[θ, 2]&lt;=2 Subscript[θ, 1]+Subscript[θ, 2],
Subscript[θ, 2]]</pre>

<p style="text-align:center;">
  $$ theta _1in text{Reals}&&theta _2geq 0 $$.
</p>

<p style="text-align:left;">
  Then, $$left{left{theta _1to 2.49667,theta _2to 5.03836right}right} $$.
</p>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->