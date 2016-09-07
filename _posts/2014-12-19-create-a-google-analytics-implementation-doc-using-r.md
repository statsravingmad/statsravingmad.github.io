---
title: Create a Google Analytics implementation doc using R
author: manos_parzakonis
layout: post
permalink: "/create-a-google-analytics-implementation-doc-using-r/"
excerpt: A post on how to fast document the setting of your GA properties
comments: true
categories:
  - measure
tags:
  - analytics
  - documentation
  - excel
  - google
  - R
published: true
share: true
---

{% include toc title="Unique Title" icon="file-text" %}

*You can get the code of post and a bit more at Github : <a href="https://github.com/IronistM/R_Google_Analytics_Doc" title="R_Google_Analytics_Doc" target="_blank">R_Google_Analytics_Doc</a>*

One of most frustrating things that you can face during a day is someone to ask you to give her the definitions of the funnel that someone&nbsp;configured a couple of years ago for 10 product categories. The only thing that can help you in this is to have an **Implementation document**. Oh, you bet you have it updated! (*side-note*: with the spread of Google Drive people tend to create documents for fun, however they rarely get to update them or even revisit them).

God, you definitely need to have written for each account you handle

  1. What you are measuring for
  2. What the configuration for the views are
  3. What is that special thing you have opted in for this view?

#### How we gonna do this fast?

After you run this code, you’ll have an Excel file that has all the following configuration point noted down

  1. Accounts
  2. Web Properties
  3. Views
  4. Goals
  5. Segments
  6. Custom Data Sources

The utility that enables us to get 1-5 is the [Google Analytics Management API][1].

The Management API (v3) exposes multiple Google Analytics configuration entities. Most are organized hierarchically:

  * Each user&#8217;s Account can have one or more Web Properties, each of which can have one or more Views (Profiles) or Custom Data Sources.
  * Each user&#8217;s Account can also have one or more Filters.
  * Views (Profiles) can have one or more Goals, Unsampled Reports or Experiments, and Custom Data Sources can have one or more Uploads. Unsampled Reports are available for Google Analytics Premium customers only.
  * There are also entities that allow users to manage user permissions by creating a User Link at the Account, Web Property, or View (Profile) level.
  * Similarly, AdWords Links can be constructed at the Web Property level.
  * Filters can be connected to a particular View (Profile) with a Profile Filter Link. A user can also define Segments, which are not hierarchically related to any of the other entities. This diagram represents the parent-child relationships among the entities:

![Google Analytics Management API][2]

#### Limitations of the Management API

We cannot get some interesting stuff with the API

  1. Channel groupings
  2. Brand keywords
  3. Enhanced eCommerce settings
  4. Events
  5. Custom dimensions/metrics

to name a few.

#### Another R Google Analytics API package&#8230;

Let&#8217;s go on and load a new package that enables us to connect to the Google Analytics various APIs, [RGA][3], all capitals since there is another package with lowercase [rga][4].

I will not go through the details on how to get authenticated on Google Console as it is well explained in the package repository.

The main functions we will need to create our implementation doc are

  1. `get_accounts()`
  2. `get_profiles()`
  3. `get_segments()`
  4. `get_webproperties()`
  5. `get_custom_sources()`
  6. `get_goals()`

#### Off to the real thing!

In the lines below we load the package and pass the `client.id` and `client.secret` to get authorised in the API. Once we done this we can get the `views` (or `profiles` as we used to call them few months ago).


~~~ r
# Initiate RGA
# load package
library(RGA)

# Connect to API
# get access token
ga_token

# Get the accountIDs
# get the unique accountIDs
accounts <- head(accounts)
[1] "183263" "431217" "4476693" "7369781" "22603640" "27175492"
~~~

Now, that we have the unique `accountIds` we can iterate over them (say in a `for()` loop) and get the elements needed.

We will create an excel file for each account we have since this is the standard file businesses are using. We could also change this to work on a website (URL) level but let&#8217;s make the assumption that we have things organised under one `accountIds` for each client. I will wrap it with an introductory sheet that you can edit to show more of your documentation style. It will be as plain as

> This is a technical report of your Google Analytics Implementation  
> Updated at : D/M/YYYY h:mm

The code to produce the result of this process is below

~~~ r
# Start getting the information! ------------------------------------------
require("dataframes2xls")

# Add an intro sheet
intro <- rbind("This is a technical report of your Google Analytics Impementation",paste("Updated at :", date()))

for (i in 1:length(accounts))
{
  tmp_goals <- get_goals(account.id = accounts[i])
  tmp_profiles <- get_profiles(account.id = accounts[i])
  tmp_webproperties <- get_webproperties(account.id = accounts[i])
  tmp_segments <- get_segments()
  tmp_custom_sources <- get_custom_sources()
  # Let's write the results to xls files
  write.xls(c(intro,tmp_webproperties,tmp_profiles,tmp_goals,tmp_segments,tmp_custom_sources), file=paste0("ga_doc_",websiteUrl[i],".xls"))
}
~~~

#### Is that all to document for?

Well if you have everything near your hands you can do something for yourself as well. Like getting all data together and trying to find if you have anything mis-configured (or&#8221;forget&#8221;). You can do this easily by creating a master table of all `accountIds` as in the following lines

~~~ r
# We will use gdata to read the excel files
require("gdata")
# Now we can merge them to run some
# diagnostics. Taken from
# http://psychwire.wordpress.com/2011/06/03/merge-all-files-in-a-directory-using-r-into-a-single-dataframe/
setwd("./reports")

# this list the files and filters Excel files
file_list <- list.files(pattern='.*\\.xls')

for (file in file_list)
{
  # if the merged dataset doesn't exist, create it
  if (!exists("dataset"))
  {
    dataset <- read.xls(file, sheet = "Sheet2") # this is the Goals sheet
  }
  # if the merged dataset does exist, append to it
  if (exists("dataset"))
  {
    temp_dataset <-read.xls(file, sheet = "Sheet2")
    dataset<-rbind(dataset, temp_dataset)
    rm(temp_dataset)
  }
}

# Example run
# > head(dataset)
~~~

Now, with a simple plot we can find things that are going wrong (eg missing e-commerce setting OFF where it should be ON).

#### The end is the beginning

This covers only the basics of a Google Analytics Documentation. There are a lot of stuff you might consider adding to this document like :

  1. The actual business questions that the implementation is aiming at
  2. KPIs you are trying to optimize for
  3. Definitions of the technical piece of tracking (passing value definitions, when an event is triggered etc)
  4. A sheet with Change history. You can get this information from the Google Analytics Admin console.

 [1]: https://developers.google.com/analytics/devguides/config/mgmt/v3/
 [2]: https://developers.google.com/analytics/images/gaManagementModel.png
 [3]: https://bitbucket.org/unikum/rga/overview
 [4]: https://github.com/skardhamar/rga
