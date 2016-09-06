---
title: Download your Facebook photos using R
author: manos_parzakonis
layout: post
permalink: /download-your-facebook-photos-using-r/
aktt_notify_twitter:
  - no
bfa_virtual_template:
  - single
categories:
  - Digital Life
tags:
  - API
  - facebook
  - pictures
  - R
---
<p style="text-align: justify;">
  So tonight I wanted to download all my Facebook pictures. For some reason the zip file was corrupted each of the 3 times I downloaded, so I remembered that some time ago I was playing around with an R project named Facebook Data-Mining. The project is conveniently located at github and you can access it <a href="https://github.com/datamgmt/facebook-data-mining" target="_blank">here</a>. <em>Looking at the  project description you realize that you can do much more with the project.</em>
</p>

<p style="text-align: justify;">
  First of all you need an Access Token that you can get from https://developers.facebook.com/tools/explorer. After you get it navigate to the <a href="https://github.com/datamgmt/facebook-data-mining/blob/master/AccessToken.R" target="_blank">AccessToken.R</a> file.Copy and paste the following as I added here the Facebook ID definition
</p>

<pre lang="rsplus"># Get a Facebook Graph API Explorer Access Token
# Go to 'https://developers.facebook.com/tools/explorer', 
# login and click "Get access token"
# Your ID is located first in the output of the Graph Explorer,it's all numbers
# Store your access token here:

me &lt;- "PasteYourFacebookIDHere"
access.token &lt;- "PasteYourAccessTokenHere"</pre>

<p style="text-align: justify;">
   Now go and source the <a href="https://github.com/datamgmt/facebook-data-mining/blob/master/Setup.R" target="_blank">Setup.R</a> script
</p>

<pre lang="rsplus"># Step 1: Create functions used by the demo
# This creates the 'facebook' function as described at
# http://romainfrancois.blog.free.fr/index.php?post/2012/01/15/Crawling-facebook-with-R
# and other functions to create initials, etc.
cat("Step 1: Create functions used by the demo","n")
source("Functions.R")

# Step 2: Run the requirements script
# This installs (if required) and loads the mandatory libaries
cat("Step 2: Run the requirements script","n")
source("Requirements.R")</pre>

Finally, run the following excerpt from the <a href="https://github.com/datamgmt/facebook-data-mining/blob/master/Photos.R" target="_blank">Photos.R</a> script

<pre lang="rsplus"># Create a "photos" directory (Warnings ignored so if it 
# already exists it will safely continue)

# Create a "photos" directory or call it as you like it
dir.create( "photos", showWarnings = FALSE )

# Create a directory for the individual's photos
dir.create( paste("photos",individual.id, sep="/"), showWarnings = FALSE )

# Download each of the individuals photos
for (i in 1:length(individual.photos.url)) {
download.file(individual.photos.url[i], individual.photos.file[i])
}</pre>

Then R will output a bunch of download streams, like this and inside the working directory you will find a photos folder and a Facebook subfolder  
[<img class="aligncenter size-full wp-image-2112" title="facebook-photos-download-using-R" alt="" src="http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2012/09/facebook-photos-download-using-R.jpg?resize=768%2C391" data-recalc-dims="1" />][1]

*Note* : It is likely that you will get the known error when a connection to Facebook via R is made

<pre lang="rsplus">Error in function (type, msg, asError = TRUE)  : 
  SSL certificate problem, verify that the CA cert is OK. Details:
error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed</pre>

<p lang="rsplus">
  The solutions to this are outlined in the <a title="A tiny RCurl headache" href="http://www.statsravingmad.com/blog/statistics/a-tiny-rcurl-headache/" rel="bookmark">A tiny RCurl headache</a> post.
</p>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->

 [1]: http://i0.wp.com/www.statsravingmad.com/blog/wp-content/uploads/2012/09/facebook-photos-download-using-R.jpg