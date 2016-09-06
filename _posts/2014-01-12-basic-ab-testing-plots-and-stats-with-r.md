---
title: Basic A/B Testing plots and stats with R
author: manos_parzakonis
layout: post
permalink: /basic-ab-testing-plots-and-stats-with-r/
excerpt: "Start analysing your A/B tests with R"
bfa_virtual_template:
  - single
wp_cta_content_placement:
  - off
wp_cta_slide_out_alignment:
  - right
wp_cta_slide_out_reveal:
  - 100
wp_cta_slide_out_speed:
  - 1
wp_cta_slide_out_keep_open:
  - no
wp_cta_popup_timeout:
  - 7
wp_cta_popup_cookie:
  - yes
wp_cta_popup_cookie_length:
  - 7
wp_cta_popup_pageviews:
  - 0
categories:
  - measure
  - statistics
tags:
  - ab test
  - ggplot
  - google analytics
  - plots
  - R
---
Have you used <a href="https://support.google.com/analytics/answer/1745147?hl=en&ref_topic=1745207&rd=1" target="_blank">Google Analytics Content Experiments</a>?

I&#8217;ve been using it lately and the most useful thing is that all data are integrated into the API to do what you want to do except for the standard reports. So, you can get them into your favorite data analysis tool and get more things done. In the following I simulate an example for an e-commerce site, so I am interested in checking the next (among others)

  * did the proposed variation increased the Funnel entrances?
  * did the feature uplift the Average Order Value (AOV) ?
  * did the conversion rate (sales wise) improved?

We can easily accomplish this using the Google Analytics API and R. We have found a way to get data off Google Analytics using R previously  (see : [Google Analytics + R = FUN!][1])

<p style="text-align: center;">
  <a href="http://i2.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2014/01/ab.test_.comparison.Rplot_.jpeg"><img class="aligncenter  wp-image-3776" alt="ab.test.comparison.Rplot" src="http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2014/01/ab.test_.comparison.Rplot_-300x198.jpeg?resize=400%2C198" data-recalc-dims="1" /></a>
</p>

<div>
  <p>
    The code that gets the above chart into a presentation of yours is hosted over at <a title="GitHub" href="https://github.com/IronistM" target="_blank">GitHub</a>. We source the multiplot.r file in order to be able to add multiple plot of the ggplot kind within a loop (see <a title="Multiple graphs on one page (ggplot2)" href="http://www.cookbook-r.com/Graphs/Multiple_graphs_on_one_page_(ggplot2)/" target="_blank">this</a>)
  </p>
</div>

<table data-pjax="">
  <tr>
    <td>
    </td>

    <td>
      <a title="Go to parent directory" href="https://github.com/IronistM/ab_testing_plots" rel="nofollow">..</a>
    </td>

    <td>
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td>
    </td>

    <td>
      <a id="38b50325c6cc23cdad060e07e2c66204-b43a3d09515cbdb0fbd8e7b16e7ae78de5bf3346" title="initiate_RGoogleAnalytics.r" href="https://github.com/IronistM/ab_testing_plots/blob/master/Scripts/initiate_RGoogleAnalytics.r">initiate_RGoogleAnalytics.r</a>
    </td>

    <td>
      <a title="First commit" href="https://github.com/IronistM/ab_testing_plots/commit/672604fa2327dbad04f1e68d78661931cc7c0e12" data-pjax="true">First commit</a>
    </td>

    <td>
      <time title="2014-01-12 10:37:41" datetime="2014-01-12T10:37:41-08:00"> </time>
    </td>
  </tr>

  <tr>
    <td>
    </td>

    <td>
      <a id="a8f8b9f3be271ccd1f8204bcf9b61815-34d09fc6916089d506b108e9541f178da5048e14" title="multiplot.r" href="https://github.com/IronistM/ab_testing_plots/blob/master/Scripts/multiplot.r">multiplot.r</a>
    </td>

    <td>
      <a title="First commit" href="https://github.com/IronistM/ab_testing_plots/commit/672604fa2327dbad04f1e68d78661931cc7c0e12" data-pjax="true">First commit</a>
    </td>

    <td>
      <time title="2014-01-12 10:37:41" datetime="2014-01-12T10:37:41-08:00"> </time>
    </td>
  </tr>

  <tr>
    <td>
    </td>

    <td>
      <a id="dc314d8373066469c190bcde8cd58aaf-33297ffdb97b7af3d9e29066d42f00e06c36ea3e" title="plot_experiments_across_profiles.r" href="https://github.com/IronistM/ab_testing_plots/blob/master/Scripts/plot_experiments_across_profiles.r">plot_experiments_across_profiles.r</a>
    </td>

    <td>
      <a title="First commit" href="https://github.com/IronistM/ab_testing_plots/commit/672604fa2327dbad04f1e68d78661931cc7c0e12" data-pjax="true">First commit</a>
    </td>

    <td>
      <time title="2014-01-12 10:37:41" datetime="2014-01-12T10:37:41-08:00"> </time>
    </td>
  </tr>
</table>

&nbsp;

 [1]: http://www.statsravingmad.com/blog/measure/google-analytics-r-fun/#comment-12398
