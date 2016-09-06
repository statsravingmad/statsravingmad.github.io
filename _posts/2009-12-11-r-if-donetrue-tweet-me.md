---
title: 'R> if (done=TRUE) tweet me!'
author: manos_parzakonis
layout: post
permalink: /r-if-donetrue-tweet-me/
categories:
  - statistics
tags:
  - averaging
  - bayesian
  - code
  - model
  - R
  - twitter
---
Let&#8217;s say that you&#8217;re fitting a cumbersome model so time is not to waste over a PC staring at the screen half anxious-half bored&#8230;

Then, you can always leave and go on with meetings and all your daily routine and have R notify you the results! How?

We will illustrate the situation above using some Bayesian Model Averaging code adapted by [Martin Feldkircher][1] & [Stefan Zeugner][2]. You should download the [code][3] and source everything in R except for the example in the end (after the definition of the functions!).

<pre>#The code to get a model
fls.data=read.table(url("http://feldkircher.gzpace.net/links/fls_data_adj.txt"))

data.M=as.matrix(fls.data)
K=ncol(dataM)-1  # nr. of regressors

# this setting corresponds to a uniform prior on the model space (prior.msize=K/2 and theta="fix")
# and the ric specification since K^2&gt; N (with N the nr. of observations) as suggested by fls
model.ric=fls(X.data=data.M,burn=60000,iter=700000,g=(1/K^2),nmodel=100,theta="fix",prior.msize=K/2,logfile=T,mcmc="bd",start.value=rep(0,K),beta.save=T)
</pre>

This is gonna take s o m e time (really!), so you could let R working and go out for a cup of coffee (typical of Greek people!). Add the following at the end of the above code.

<pre>library(twitteR)
sess &lt;- initSession('myUser', 'myPass') # Set your user account info
ns &lt;- updateStatus('A model waits for you @ home ;)', sess)
</pre>

Would you really care enough to check whether the fit is done when outside?

 [1]: http://feldkircher.gzpace.net/
 [2]: http://www.zeugner.eu/
 [3]: http://feldkircher.gzpace.net/links/bma_20081105_homepageVersion.r