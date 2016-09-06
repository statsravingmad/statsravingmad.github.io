---
title: 'Read a new book&#8230;'
author: manos_parzakonis
layout: post
permalink: /read-a-new-book/
aktt_notify_twitter:
  - yes
  - yes
aktt_tweeted:
  - 1
categories:
  - infos
  - probability
  - statistics
tags:
  - book
  - code
  - free
  - R
---
From the book website :

> IPSUR stands for Introduction to Probability and Statistics Using R,  
> ISBN: 978-0-557-24979-4, which is a textbook written for an  
> undergraduate course in probability and statistics. The approximate  
> prerequisites are two or three semesters of calculus and some linear  
> algebra in a few places. Attendees of the class include mathematics,  
> engineering, and computer science majors. 

Now, there is a new way to read R books (anyway, new to me!)

<pre lang="rsplus" line="1">install.packages("IPSUR", repos="http://cran.r-project.org")
library(IPSUR)
read(IPSUR)</pre>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->