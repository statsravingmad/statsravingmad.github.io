---
title: Google Analytics + R = FUN!
author: manos_parzakonis
layout: post
permalink: /google-analytics-r-fun/
excerpt: "Free your Google Analytics data from the UI and analyse them with R. New pathways upon us!"
bfa_virtual_template:
  - single
categories:
  - measure
tags:
  - code
  - google analytics
  - package
  - R
  - web analytics
---
<p style="text-align: justify;">
  The scope of this post it to show how simple it is to get data out of the Google Analytics and create your own reports (that you hope that they can be semi-automated at least) and you favourite statistical graphs (those that GA is currently missing). As you already know R is a favourite tool to me, so this will be the main tool to get the data, reshape them and depict them. You will need elementary knowledge of the R language and in the end you&#8217;ll soon relaize that google search is more than 40% of the code polishing stuff your code will ever need&#8230;
</p>

### R packages

<p style="text-align: justify;">
  There are two packages (or libraries) that connect to the Google Analytics API and return data to you, the older one is <a title="RGoogleAnalytics" href="https://code.google.com/p/r-google-analytics/" target="_blank">RGoogleAnalytics</a> and the new champion is <a title="rga" href="https://github.com/skardhamar/rga" target="_blank">rga</a>. Both are excellent and I&#8217;ve used both at all occasions. rga seems a bit nicier but RGoogleAnalytics is certainly more robust and works under all occasions. Apart from the core, I will use <a href="http://projecttemplate.net/" target="_blank">ProjectTemplate</a> for my personal organisation (you won&#8217;t see it however) and ggplot2 for graphics.
</p>

#### Google Authentication

<p style="text-align: justify;">
  First of all go to <a href="https://code.google.com/apis/console/" target="_blank">Google&#8217;s API Console</a> and create a new API application after you make sure that you have Google Analytics Service enabled.
</p>

<h4 style="text-align: justify;">
  <Pause>
</h4>

Because of the lengthy script that would ruin the flow of the post I have created a Github repo where all scripts reside [<a href="https://github.com/IronistM/R_Google_Analytics/archive/master.zip" target="_blank">zip</a>]

<div id="js-repo-pjax-container" data-pjax-container="">
  <div id="slider">
    <div data-permalink-url="/IronistM/R_Google_Analytics/tree/c8319b20b1997656f11c5f688a5961a72fa98294" data-title="IronistM/R_Google_Analytics · GitHub" data-type="tree">
      <table cellspacing="0" cellpadding="0">
        <tr>
          <td>
          </td>

          <td>
            <a id="04c6e90faac2675aa89e2176d2eec7d8-65af36bfa95ac157e4ff24e36808d9b36fef0845" title="README.md" href="https://github.com/IronistM/R_Google_Analytics/blob/master/README.md">README.md</a>
          </td>

          <td>
            <time title="2013-05-05 09:53:12" datetime="2013-05-05T09:53:12-07:00"> </time>
          </td>

          <td>
            <a href="https://github.com/IronistM/R_Google_Analytics/commit/0972aff65863d8647b9d7c740a515c6ab685eaee">Update README.md</a> [<a href="https://github.com/IronistM" rel="author">IronistM</a>]
          </td>
        </tr>

        <tr>
          <td>
          </td>

          <td>
            <a id="6a4d0b6f0ca176c2f1e62e2c594f2377-7480f8e9a53d1c940dca101a987d2ddaaedbd28f" title="rga_blog_speed_analytics.R" href="https://github.com/IronistM/R_Google_Analytics/blob/master/rga_blog_speed_analytics.R">rga_blog_speed_analytics.R</a>
          </td>

          <td>
            <time title="2013-05-05 09:57:53" datetime="2013-05-05T09:57:53-07:00"> </time>
          </td>

          <td>
            <a href="https://github.com/IronistM/R_Google_Analytics/commit/94bc331a9fa6d45cf7c0d5f38c888577b90c006c">Update rga_blog_speed_analytics.R</a> [<a href="https://github.com/IronistM" rel="author">IronistM</a>]
          </td>
        </tr>

        <tr>
          <td>
          </td>

          <td>
            <a id="7d0e51bf05f71e36bc8ae2580f837dac-79d8069d1f459b99a59c4b64a34335a65eb262d6" title="rga_get_profiles.R" href="https://github.com/IronistM/R_Google_Analytics/blob/master/rga_get_profiles.R">rga_get_profiles.R</a>
          </td>

          <td>
            <time title="2013-05-05 09:52:06" datetime="2013-05-05T09:52:06-07:00"> </time>
          </td>

          <td>
            <a href="https://github.com/IronistM/R_Google_Analytics/commit/247a195ad2dcf473b0c8e6961d5f4663f8feac33">Create rga_get_profiles.R</a> [<a href="https://github.com/IronistM" rel="author">IronistM</a>]
          </td>
        </tr>

        <tr>
          <td>
          </td>

          <td>
            <a id="47c29b2b16f6f322f962240a21f0e482-3eca2d6bee722d79cf7ae0bdc5a2e15fb91a2951" title="rga_graph_speed_analytics.R" href="https://github.com/IronistM/R_Google_Analytics/blob/master/rga_graph_speed_analytics.R">rga_graph_speed_analytics.R</a>
          </td>

          <td>
          </td>

          <td>
            <a href="https://github.com/IronistM/R_Google_Analytics/commit/c8319b20b1997656f11c5f688a5961a72fa98294">Update rga_graph_speed_analytics.R</a> [<a href="https://github.com/IronistM" rel="author">IronistM</a>]
          </td>
        </tr>

        <tr>
          <td>
          </td>

          <td>
            <a id="1c10d45d45b329bc7ac823f5a0d91783-25efa6f3380fda782e82257f3a2602a480721179" title="rga_initiate_API_connection.R" href="https://github.com/IronistM/R_Google_Analytics/blob/master/rga_initiate_API_connection.R">rga_initiate_API_connection.R</a>
          </td>

          <td>
            <time title="2013-05-05 09:50:40" datetime="2013-05-05T09:50:40-07:00"> </time>
          </td>

          <td>
            <a href="https://github.com/IronistM/R_Google_Analytics/commit/3fd6af682777bdde6b9316b325d0fc9e891a8f1a">Create rga_initiate_API_connection.R</a> [<a href="https://github.com/IronistM" rel="author">IronistM</a>]
          </td>
        </tr>
      </table>
    </div>
  </div>
</div>

#### Ready,Set,Goooo!

<p style="text-align: justify;">
  Now, that the API access is set-up in the one side,we should make the connection to R. You should already know that RCurl is a bit tricky as I have oulined in the <a id="mfa92" href="http://www.statsravingmad.com/blog/statistics/a-tiny-rcurl-headache/#comment-12049">A tiny RCurl headache</a> note. The solution proposed there is applied here as well. note that this issue will be solved in the next release of rga. On the other hand RGoogleAnalytics seems to be already on the spot. Have in mind that using
</p>

<pre>ssl.verifypeer = FALSE</pre>

<p style="text-align: justify;">
  isn&#8217;t the most secure way to use network communication in R.  You can use the following to create a connection to the API [<a id="1c10d45d45b329bc7ac823f5a0d91783-25efa6f3380fda782e82257f3a2602a480721179" title="rga_initiate_API_connection.R" href="https://github.com/IronistM/R_Google_Analytics/blob/master/rga_initiate_API_connection.R">rga_initiate_API_connection.R</a>] This is heavily copy-pasted from Randy Zwitch&#8217;s <a href="http://randyzwitch.com/r-google-analytics-api/" target="_blank">(not provided): Using R and the Google Analytics API</a> post.
</p>

<p style="text-align: justify;">
  One issue is how to get the Profile IDs. The hard way would be to go to <a title="Google Analytics Tools - Query Explorer" href="ga-dev-tools.appspot.com/explorer/" target="_blank">Query Explorer</a> and cycle through all profiles and write down the IDs that you are interested in. However, luck is all you got as there is a function that will return you all profiles that you have access with the account tied to the API access you created. (BTW, it is excellent that there is provision for access to the Management API in the rga package)
</p>

<p style="text-align: justify;">
  <!-- Missing Gist ID -->
</p>

*In the next I will **assume** that you have defined the ids that you are interested in.*

<p style="text-align: justify;">
  The main hypothesis that I want to get a taste of is whether the different post categories (eg. measure, statistics, music etc) have different load times. This will be interesting given that all categories don&#8217;t have the same burden to get loaded (images vary, youtube videos scripts). To achieve the following you will need to use a filters vector and loop over it. Give appropriate names in the page.group vector and you will be done. Note, that we have created extra metrics
</p>

<li style="text-align: justify;">
  <em><strong>e-commerce rate</strong></em> : this is not meaningful in the case of a blog, but if you are advanced in analytics you might have implemented goals as e-commerce events as B. Clifton <a href="https://support.google.com/analytics/answer/1011367?hl=en" target="_blank">suggests</a>.
</li>
<li style="text-align: justify;">
  <em><strong>bounce rate</strong> </em>: the bounce rate should be correlated to the page load
</li>
<li style="text-align: justify;">
  <em><strong>buckets of page load time </strong></em>: we use a 4 seconds range for each bucket to be consistent with the the <a href="http://apdex.org/overview.html" target="_blank">Apdex</a> standard.
</li>

<p style="text-align: justify;">
  Because I want to get a more metrics than a single query allows (11) I use another query in the loop to ge the rest and then merge them.Now, if you run all these scripts you will have a data frame like this extracted in the end of the script using the head() function
</p>

<p style="text-align: justify;">
  <!-- Missing Gist ID -->
</p>

<h4 style="text-align: justify;">
  <strong>Enough with scripting!</strong>
</h4>

<p style="text-align: justify;">
  Now, that the data are on our console we can finally get some graphs. The following histogram is the aggregated page load speed histogram of this blog. You should note that there is a significant volume of sample units that belongs to the 12-16 bucket. I have the suspicion that they also belong to a specific country group as well as the host is providing good page load timings in the US and Western Europe. (<em>Note to myself</em> : I should add the <a href="https://developers.google.com/analytics/devguides/reporting/core/dimsmets/geonetwork.html#ga:country">ga:country</a> dimension in the second query run).
</p>

<p style="text-align: center;">
  <a href="http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2013/05/Rplot.jpeg" target="_blank"><img class="aligncenter  wp-image-3692" alt="Rplot" src="http://i2.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2013/05/Rplot-300x192.jpeg?resize=400%2C252" data-recalc-dims="1" /></a>
</p>

<p style="text-align: justify;">
  OK. This is not a nice picture at all! I know that I have experimented with various analytics scripts in the last months plus in the first 3 months of 2013 I was using a significantly heavier wordpress theme but I still think the the sample is skewed by the georgaphic distribution of the readers (a new post will come soon on this!)
</p>

<p style="text-align: center;">
  <a href="http://i1.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2013/05/Rplot01.jpeg" target="_blank"><img class="aligncenter  wp-image-3698" alt="Rplot01" src="http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2013/05/Rplot01-300x192.jpeg?resize=400%2C252" data-recalc-dims="1" /></a>
</p>

<h4 style="text-align: justify;">
  Extend the script to your needs
</h4>

<p style="text-align: justify;">
  In a modification of the script above I can use a loop on the web properties that I have access , so I use R to store data and create a roll-up report in a fast way. If you are looking at the comments section of the scripts you will notice the following.
</p>

<pre># In the future we should only get data for increment dates. Don&#039;t we?
get.start.date&lt;-min(final_dataset$date)</pre>

<p style="text-align: justify;">
  I use this to incrementally query and store data in the final_dataset data frame (this will help with the sampling that I will run into the first time of running the script for a long period of time). I am pretty sure a cron thing can be streamlined here, however I have no idea on cron jobs&#8230;
</p>

<p style="text-align: justify;">
  Head now to <a href="https://github.com/IronistM/R_Google_Analytics">https://github.com/IronistM/R_Google_Analytics</a> !
</p>
