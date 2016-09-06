---
title: 'Poor man&#8217;s pairs trading&#8230;'
author: manos_parzakonis
layout: post
permalink: /poor-mans-pairs-trading/
aktt_notify_twitter:
  - yes
aktt_tweeted:
  - 1
categories:
  - statistics
tags:
  - arbitrage
  - cointegration
  - finance
  - pairs
  - R
  - statistical
  - trading
---
There is a central notion in Time Series Econometrics, [cointegration][1]. Loosely it refers to finding the long run equilibrium of two non-stationary series. As the most know non-stationary series examples comes from finance, cointegration is nowadays a tool for traders (not a common one though!). They use it as the theory behind [pairs trading][2] (aka [Statistical Arbitrage][3]).

In the following lines we use a simple pairs trading technique, studying the ratio of the two price evolution series. We use the New York versions of two of the greatest players in [ATHEX][4], [NBG][5] and [OTE][6].

<pre>stock &lt;- "NBG"
stock1 &lt;- "OTE"
start.date &lt;- "2003-10-20"
end.date &lt;- Sys.Date()
quote &lt;- paste("http://ichart.finance.yahoo.com/table.csv?s=",
 stock,
 "&a=", substr(start.date,6,7),
 "&b=", substr(start.date, 9, 10),
 "&c=", substr(start.date, 1,4),
 "&d=", substr(end.date,6,7),
 "&e=", substr(end.date, 9, 10),
 "&f=", substr(end.date, 1,4),
 "&g=d&ignore=.csv", sep="")
quote1 &lt;- paste("http://ichart.finance.yahoo.com/table.csv?s=",
 stock1,
 "&a=", substr(start.date,6,7),
 "&b=", substr(start.date, 9, 10),
 "&c=", substr(start.date, 1,4),
 "&d=", substr(end.date,6,7),
 "&e=", substr(end.date, 9, 10),
 "&f=", substr(end.date, 1,4),
 "&g=d&ignore=.csv", sep="")
dataNBG.l &lt;- read.csv(quote, as.is=TRUE)
dataOTE.l &lt;- read.csv(quote1, as.is=TRUE)
X2=dataOTE.l[order(dataOTE.l$Date),];Y2=dataNBG.l[order(dataNBG.l$Date),]
</pre>

<!--more-->

  
Now, let&#8217;s plot the ratio of the two. We locate a traing opportunity when there is an exceedence of the 2sigmas. Then we go short on the OTE and long on the NBG and close the positions on when the ratio is approaching the mean.

<pre>par(mfrow=c(1,1),fg = gray(0.7), bty="7")
plot(X2$Close,ylim=c(0,max(X2$Close)),ylab="Price",type="l")
lines(Y2$Close,col="red");grid()
lines(X2$Close/Y2$Close,col="green")

pairt=X2$Close/Y2$Close
abline(h=mean(pairt)+c(-2:2)*sd(pairt),col=c("black","purple"))
abline(h=mean(pairt),col="red")

<a href="http://www.statsravingmad.com/blog/wp-content/uploads/2010/04/NBG.OTE_.jpeg"></a></pre>

<p style="text-align: center;">
  <a href="http://i1.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/04/NBG.OTE_1.jpeg"><img class="alignnone size-medium wp-image-1249" title="NBG.OTE" src="http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2010/04/NBG.OTE_1-300x300.jpg?resize=527%2C399" alt="" data-recalc-dims="1" /></a>
</p>

<p style="text-align: left;">
  Looking at the above there was a opportunity to start a pair trading at the end of the 2009&#8230;
</p>

<pre style="text-align: left;">X2$Date[which(abs(pairt-mean(pairt))&gt;2*sd(pairt))]
<span style="color: #339966;">#   [1] "2008-11-19" "2008-11-20" "2008-11-21" "2008-12-01" "2008-12-02"
#   [6] "2008-12-03" "2008-12-04" "2008-12-05" "2008-12-08" "2008-12-09"
#  [11] "2008-12-10" "2008-12-11" "2008-12-12" "2008-12-15" "2008-12-16"
#  [16] "2008-12-17" "2008-12-18" "2008-12-19" "2008-12-22" "2008-12-23"
#  [21] "2008-12-24" "2008-12-26" "2008-12-29" "2008-12-30" "2008-12-31"
#  [26] "2009-01-02" "2009-01-05" "2009-01-06" "2009-01-07" "2009-01-08"
#  [31] "2009-01-09" "2009-01-12" "2009-01-13" "2009-01-14" "2009-01-15"
#  [36] "2009-01-16" "2009-01-20" "2009-01-21" "2009-01-22" "2009-01-23"
#  [41] "2009-01-26" "2009-01-27" "2009-01-28" "2009-01-29" "2009-01-30"
#  [46] "2009-02-02" "2009-02-03" "2009-02-04" "2009-02-05" "2009-02-06"
#  [51] "2009-02-09" "2009-02-10" "2009-02-11" "2009-02-12" "2009-02-13"
#  [56] "2009-02-17" "2009-02-18" "2009-02-19" "2009-02-20" "2009-02-23"
#  [61] "2009-02-24" "2009-02-25" "2009-02-26" "2009-02-27" "2009-03-02"
#  [66] "2009-03-03" "2009-03-04" "2009-03-05" "2009-03-06" "2009-03-09"
#  [71] "2009-03-10" "2009-03-11" "2009-03-12" "2009-03-13" "2009-03-16"
#  [76] "2009-03-17" "2009-03-18" "2009-03-19" "2009-03-20" "2009-03-23"
#  [81] "2009-03-24" "2009-03-25" "2009-03-26" "2009-03-27" "2009-03-30"
#  [86] "2009-03-31" "2009-04-01" "2009-04-02" "2009-04-03" "2009-04-06"
#  [91] "2009-04-07" "2009-04-08" "2009-04-09" "2009-04-13" "2009-04-14"
#  [96] "2009-04-15" "2009-04-16" "2009-04-17" "2009-04-20" "2009-04-21"
# [101] "2009-04-22" "2009-04-23" "2009-04-24" "2009-04-27"</span></pre>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->

 [1]: http://en.wikipedia.org/wiki/Cointegration
 [2]: http://en.wikipedia.org/wiki/Pairs_trade
 [3]: http://en.wikipedia.org/wiki/Statistical_arbitrage
 [4]: http://www.ase.gr/
 [5]: http://www.nbg.gr/
 [6]: http://www.ote.gr/portal/page/portal/OTEGR/OTEMainPage