---
title: 'A bayesian cinefeel&#8230;(*)'
author: manos_parzakonis
layout: post
permalink: /a-bayesian-cinefeel/
categories:
  - infos
  - statistics
tags:
  - bayesian
  - conjugate
  - movies
  - prior
---
How [imbd][1] ranks movies?

> *The formula for calculating the Top Rated 250 Titles gives a **true Bayesian estimate**:*

> <p style="text-align:center;">
>   <em><img src="//s0.wp.com/latex.php?latex=WR%3Dfrac%7Bnu+%7D%7Bnu+%2Bm%7DR%2Bfrac%7Bm%7D%7Bnu+%2Bm%7DC&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="WR=frac{nu }{nu +m}R+frac{m}{nu +m}C" title="WR=frac{nu }{nu +m}R+frac{m}{nu +m}C" class="latex" /></em>
> </p>

> * where:*

> *<img src="//s0.wp.com/latex.php?latex=R&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="R" title="R" class="latex" /> = average for the movie (mean) = (Rating)*

> *<img src="//s0.wp.com/latex.php?latex=nu&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="nu" title="nu" class="latex" /> = number of votes for the movie = (votes)*

> *<img src="//s0.wp.com/latex.php?latex=m&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="m" title="m" class="latex" /> = minimum votes required to be listed in the Top 250 (currently 1500)*
> 
> *<img src="//s0.wp.com/latex.php?latex=C&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="C" title="C" class="latex" /> = the mean vote across the whole report (currently 6.9)*
> 
> * for the Top 250, only votes from regular voters are considered.* ([source][2])

Now that&#8217;s something unexpected! Going further than the simple arithmetic mean is something exciting, right?

The formula is the well-known decomposition of the posterior mean -distributed a priori as normal- compromising the prior guess (*<img src="//s0.wp.com/latex.php?latex=C&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="C" title="C" class="latex" />*) and the data (*<img src="//s0.wp.com/latex.php?latex=R&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="R" title="R" class="latex" />*) weighted by the sample (*<img src="//s0.wp.com/latex.php?latex=nu&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="nu" title="nu" class="latex" />*) and the pretend-to-be initial sample (*<img src="//s0.wp.com/latex.php?latex=m&#038;bg=ffffff&#038;fg=000&#038;s=0" alt="m" title="m" class="latex" />*) under the conjugate prior setting.

 [1]: http://www.imdb.com/
 [2]: http://www.imdb.com/chart/top