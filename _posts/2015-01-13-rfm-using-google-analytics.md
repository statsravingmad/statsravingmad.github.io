---
title: RFM Using Google Analytics
author: manos_parzakonis
layout: post
permalink: "/google-analytics-rfm/"
excerpt: A post on how to start using classic marketing tactics on your Digital Analytics Powerhouse
comments: true
categories:
  - measure
tags:
  - analytics
  - rfm
  - crm
  - google
  - R
published: true
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

Google Analytics is no more a tool for measuring hits, as of this announcement ([Remarketing audiences in Google Analytics](https://support.google.com/analytics/answer/2611268)) 2 years ago. It is a Marketing tool that marketeers with little sense can utilize to gain a lot and make their campaigns work-flow smarter and more effective!

One way that can make remarketing more effective is continuous slicing & dicing of members of a remarketing list. One smart way of doing this is by classifying your customers. One way of implementing this is the logic below :

    A customer who has visited your site Recently (R) and Frequently (F) and created
    a lot of Monetary Value (M) through purchases is much more likely to visit and
    buy again.
    And, a high Recency / Frequency / Monetary Value (RFM customer who stops visiting is a customer
    who is finding alternatives to your site.

    It makes sense, doesn’t it?

  *source : [Jim Novo](http://www.jimnovo.com/RFM-tour.htm)*

#### How to get User level data in Google Analytics

  It is sufficiently easy to get data on a user in Google Analytics nowadays, utilizing even the Client ID feature that Universal Analytics is offering with a simple code addition in the tracking code:

~~~ javascript
ga(function(tracker) {
    var rid = tracker.get('clientId');
    ga('set', 'dimension6', rid);
  });
~~~

  Keep in mind that by default, Google Analytics assigns each device a unique Client ID, and considers each unique Client ID as a unique user in your reports. So, in the following we will look at device specific data but the same is seemlingly expanded to the case that you have roll out your own User ID you will have an RFM. The code above stores the Client ID into a Custom Dimension in order to be something we can query.

#### The mechanics of RFM

  Customers are ranked based on their R, F, and M characteristics, and assigned a "score" representing this rank.  Assuming the behavior being ranked (purchase, visit) using RFM has economic value, the higher the RFM score, the more profitable the customer is to the business now and in the future.  High RFM customers are most likely to continue to purchase and visit, AND they are most likely to respond to marketing promotions.  The opposite is true for low RFM score customers; they are the least likely to purchase or visit again AND the least likely to respond to promotions.

##### Get the data

  We covered in previous posts how you can get the data programmatically however what we will need can be pulled with a Custom Report with the following dimensions

  `Cliend Id ; Date ; TransactionId ; Revenue`


You can get the report from the Solution Gallery [here](https://www.google.com/analytics/web/template?uid=ePSpvoO0RRWsszkIBrIv8Q).

You can also get the data using RGA

~~~ r
library(RGA)
ga_data <- get_ga(profile.id = XXXXXXXX, start.date = "300daysAgo",
end.date = "yesterday", metrics = "ga:transactionRevenue",
dimensions = "ga:transactionId,ga:date,ga:dimensionXX")
~~~

  Now, that we have the data in our hands we need to transform them into a data frame that conform to an RFM dataset

  1. Remove the duplicate records with the same Client ID
  2. Find the most recent date for each ID and calculate the days to the endDate, to get the Recency data
  3. Calculate the quantity of translations of a customer, to get the Frequency data
  4. Sum the amount of money a customer spent and divide it by Frequency, to get the average amount per transaction, that is the Monetary data.

#### easyRFM

  There is a really interesting package for RFM analysis in R, [easyRFM](https://github.com/hoxo-m/easyRFM). I have forked the repository and added a few functions like the above to have them bundled in one place.

  We can get the scores for each component of RFM using the following line(s) of code.

~~~ r
result <- rfm_auto(ga_data,id="dimensionXX",payment="transactionRevenue",
date="date",date_format = "%Y%m%d")
~~~

  Note that we need to provide the format of our date variable as it is different from our (Google Analytics API returns ISO dates).
  Before moving on, sneak peak into the object that the rfm_auto created

~~~ r
> summary(result)
Length Class      Mode
rfm            7      data.frame list
breaks         4      -none-     list
classes        4      -none-     list
get_table      1      -none-     function
get_sliced_rfm 1      -none-     function
~~~
  Technically the result we have constructed will be a list. This is important to know for calling them ;)

  A lot of the domain expertise that you will need to apply to this model is creating categories.

  We can get a gist of our data using a summary view of our main data frame with some plots.

~~~ r
# We will need a multiple plot to save up some space on this post,
# ie multiplot() from AB Testing post  
devtools::source_url("https://raw.githubusercontent.com/IronistM/ab_testing_plots/master/Scripts/multiplot.r")
# A palette with nice colours :
cbPalette <- c("#999999", "#E69F00", "#56B4E9", "#009E73", "#F0E442", "#0072B2", "#D55E00", "#CC79A7")
a <- qplot(result$rfm$FrequencyClass, xlab="Frequency Class")
b <- qplot(result$rfm$RecencyClass, xlab="Recency Class")
c <- qplot(result$rfm$MonetaryClass, xlab="Monetary Class")
multiplot(b,a,c) # To be aligned with RFM
~~~
![rfm_histograms](https://raw.githubusercontent.com/statsravingmad/statsravingmad.github.io/master/images/rfm_histograms.jpeg)

However, there is one category that is the same for each industry & vertical, new customers aka 1-1-1 as the encoding of RFM dictates. We can define other categories of interest along the lines of easyRFM tutorial for simplicity.

~~~ r
  '111_customers' <- result$get_sliced_rfm(R_slice=1, F_slice=1, M_slice=1)
  leaved_customers <- result$get_sliced_rfm(R_slice=1:2, F_slice=2:5, M_slice=4:5)
  leaving_customers <- result$get_sliced_rfm(R_slice=3:4, F_slice=4:5, M_slice=4:5)
  good_customers <- result$get_sliced_rfm(R_slice=5:6, F_slice=4:5, M_slice=4:5)
~~~

#### Applications of the RFM scoring

  It is important that this can lead us to creating specific audiences in our campaigns for nurturing, re-activating, engaging customers using the neat feature of Universal Analytics called [Data Import](https://support.google.com/analytics/answer/4524584?hl=en). Then, all this analysis will be a feedback loop over the Google Analytics server. Even better if you have implemented your own User ID and you have the data crunched over all devices then this will be a lot more powerful analysis.

###### Create user scope dimensions

  Let's assume that to map users to external data from a CRM system you can use custom dimensions to represent the CRM user and imported values in Google Analytics. This means that we query the Google Analytics API via a scheduled procedure to merge sales data to our CRM. This means that we have a lookup table with one to many relationship but we can easily dedup before uploading.

  The steps to create the dimensions are:

1. Click Custom Definitions -> Custom Dimensions -> + New Custom Dimension.
2. Name the dimensions and set the scope to User.
3. Click Create.

We will not use the dimension ga:UserId available in Google Analytics to join CRM data with a user since the primary use case for the User ID feature is to enable cross device reporting, not for data import. We will work with a custom dimension instead.

###### Create a data set

To import your user data you must create a data set. A data set is used to represent one or more external data sources in Google Analytics. A data set can only be created through the Web Interface (for now at least).

Under the property tab of the admin page perform the following steps:

1. Select Data Import.
2. Click New Data Set.
3. Select User Data for the type and click Next Step.
4. Name the data set and select at least one view (profile) and click Next Step.
5. For the Key select the `Client Id` dimension we defined above.
6. For Imported Data select the dimension `RFM Score` we created above.
7. Select an Overwrite hit data option.
8. Click Save.

###### Prepare user data for upload

Google Analytics expects user data to be uploaded in a properly formatted comma separated file (CSV). You need to make sure the user data meets these requirements before uploading.

The primary modifications and validations that you need to make to the CSV file are:

1. Rename the column headers to those recognized by Google Analytics. You can retrieve the header from the data set details page in the web interface. In our case `dimension6` stands for the `Client Id` and `dimension7` for the RFM category.
2. Add any required values that are missing.
For example our analysis may result in a csv file in the following format:

~~~
ga:dimension6,ga:dimension7
1298227728.14192,111
1302883997.14172,112
26330190.1418789,221
1057785772.14191,111
1730884285.14192,151
325837902.141911,111
681366284.141832,111
1020893480.14177,555
1138596729.14192,232
1194713427.14196,111
~~~

###### Upload user data

Once you have created your custom dimensions and data set, and prepared your user data for upload in a CSV file, you're ready to upload your content data using the [Management API](https://developers.google.com/analytics/devguides/config/mgmt/v3/mgmtReference/management/uploads/uploadData) or through the [web interface](https://support.google.com/analytics/answer/6015014). Since there is an option to simply upload a file let's make our life easier and move on with this option.

###### Act on the data!
With the component pieces in place it is now possible to analyze the results and take action. The steps needed to create a remarketing list from the imported data are:

- Create a custom report :  The easiest way to verify and evaluate the imported user data is to create a custom report
- Create a segment : Watch the users you want to target in the wealth of Google Analytics reports.
- Create a remarketing list : The ultimate action ; run a campaign!
