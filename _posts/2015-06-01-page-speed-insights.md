---
title: Monitor your Page Speed Insights score using Google Sheets
author: manos_parzakonis
excerpt: A custom function to query Google's Page Speed Insight API.
comments: true
categories:
- measure
tags:
- analytics
- insights
- API
- google apps
- google sheets
- speed

---

{% include toc title="Unique Title" icon="file-text" %}

In a previous [post](http://statsravingmad.com/a-roll-up-google-analytics-dashboard-using-google-sheets-apps-script/) we have shown how we can create a Google Analytics dashboard
using Google Sheets. In this post we will create a simple dashboard to monitor the page speed insights score as this is provided by Google's service. The end deliverable will include the option of sending an email to the developer team bench marking our performance to other competitors and similar size sites.


*Note* : This is not an totally original code from myself but rather things put together for stackoverflow & the google apps documentation ; so treat it again as a "notes" post.
 
# What is Google Apps Script

> Google Apps Script is an automation API that lets developers use
> JavaScript to interact with many of Google's products, including
> Calendar, Docs and Contacts. Using the API, developers can automate
> tasks or otherwise improve their own experience or that of other users
> while using Google products. For example, a developer might take a
> Google spreadsheet and add buttons and other interface components to
> improve means of gathering user input for calculations. Apps Scripts
> can also create new pages on Google Sites, send emails triggered by
> specific spreadsheet data and more. In addition to accessing Google
> products, the Apps Scripts also can access utility services, such as
> an XML parser and SOAP call creator.

*source* : [ProgrammableWeb](http://www.programmableweb.com/api/google-apps-script)

# What is the PageSpeed Insights API

[Page Speed Insights](https://developers.google.com/speed/pagespeed/insights) measures the performance of a webpage. The PageSpeed Score ranges from 0 to 100 points. A higher score is better and a score of 85 or above indicates that the page is performing well. It does so for mobile devices and desktop devices. It fetches the url twice, once with a mobile user-agent, and once with a desktop-user agent.

PageSpeed Insights measures how the page can improve its performance on:

- *time to above-the-fold load*: Elapsed time from the moment a user requests a new page and to the moment the above-the-fold content is rendered by the browser.
- *time to full page load*: Elapsed time from the moment a user requests a new page to the moment the page is fully rendered by the browser.

PageSpeed Insights only evaluates the network-independent aspects of page performance: the server configuration, the HTML structure of a page, and use of external resources (such as images, JavaScript & CSS). In the end the implementation of the suggestions should improve the relative performance of the page. However, since the performance of a network connection varies considerably again the absolute performance of the page will be dependent upon a user’s network connection.

## Intro to the  API
The best thing Google has done with the vast collection of APIs is to create an API playground. [Page Speed API explorer](https://developers.google.com/apis-explorer/#p/pagespeedonline/v2/ ). Please spend 5 or 10 minutes experimenting with it and you will see how rich information it offers.

## Implement a Google Apps function

Our strategy is to create a custom function that we will be able to call in a cell just like we call `=sum()`. You can read more on [Custom functions in Google Sheets](https://developers.google.com/apps-script/guides/sheets/functions) in the Google Developers site.

Without further ado...

``` Javascript
function pageSpeedInsights(url,device,filter_third_party_resources,http_secure) {

  url = url || 'www.statsravingmad.com'; // if no url is passed as argument you will get my score :)
  strategy = 'desktop' || device; // 'desktop' or 'mobile'.
  filter_third_party_resources = 'true' || filter_third_party_resources;
  http_secure = 'false' || http_secure ; // if it SSL type in "true".
  Logger.log(http_secure); // for test runs. Comment it out if you like. See logs using Ctrl + Enter.

  // Create a protocol parameter to pass to the GET URL
  switch (http_secure)  {
    case 'false':
    http_protocol = 'http://';
    break;
    case 'true':
    http_protocol = 'https://';
    break;
  }

  Logger.log(http_protocol); // for test runs. Comment it out if you like.

  var key = 'YOUR API KEY';     // Get the API key from Google Dev Console
  var api = 'https://www.googleapis.com/pagespeedonline/v2/runPagespeed?url=' + http_protocol + url
  + '&filter_third_party_resources=' + filter_third_party_resources + '&strategy=' + strategy + '&key=' + key;

  Logger.log(api); // for test runs. Comment it out if you like.
  Logger.log(url); // for test runs. Comment it out if you like.

  var response = UrlFetchApp.fetch(api, {muteHttpExceptions: true });

  var result = JSON.parse(response.getContentText()); // yeap, it is JSON

  // Example of JSON in order to formulate the score below
  // "kind": "pagespeedonline#result",
  // "id": "http://statsravingmad.com/",
  // "responseCode": 200,
  // "title": "Stats Raving Mad",
  // "ruleGroups": {
    //   "SPEED": {
      //     "score": 75
      //   }
      // },

      score = result.ruleGroups.SPEED.score;
      Logger.log(score); // for test runs. Comment it out if you like.

      return(score);
    }
```

# Create the Monitoring Sheet

  The sheet is pretty simple. Enter the domains you'd like to monitor in a column and then use the `pageSpeedInsights()` function to get the score in another cell.

  For a visual aid we can use a `sparkline()` like this :

  `=iferror(SPARKLINE(B4;{"charttype"\"bar";"max"\ 100;"color1"\ if(B4>80;"#19A347";if(B4<65;"#FF4D4D";"#FFA319"))});"")`

  If it is above 85 then everything is blue (cornflower blue!) , if it below 65 then things need fixing and in the range in-between we flag them for review.

*Note* : `\` stands for `,` when regional settings are set to`GREEK`.

![Page Speed Monitoring](https://github.com/statsravingmad/statsravingmad.github.io/blob/master/images/page-speed-insights-monitoring.png)

# Email the report
In order to mail the report we need to figure two things

  1. a way to make the refresh of the data recurring , and
  2. how to send an email using google sheets & apps script

For the first bullet we can set a trigger to run the `pageSpeedInsights` function every month.

Next, we can use the following function to create the email body and (most important) the attached pdf of the report. The core apps to use are `DriveApp` and the `MailApp`

``` Javascript
    function spreadsheetToPDF(){

      var key = 'YOUR SPREADSHEET ID';  //docid

      var index = 0;  //sheet gid / number

      var ss = SpreadsheetApp.getActiveSpreadsheet();
      var ActiveSheet = ss.getSheetByName('Sheet1');

      var year = Utilities.formatDate(new Date(), "GMT", "yyyy");
      var month = Utilities.formatDate(new Date(), "GMT", "MM");

      var filename = 'Page Speed Insights Score' + '_' + year + month + '.pdf';  //makes pdf filename

      SpreadsheetApp.flush();  //ensures everything on spreadsheet is "done"
      Logger.log("Start the pdf creation!");

      var theurl = 'https://docs.google.com/a/statsravingmad.com/spreadsheets/d/' // you can see this URL in the browser window
      + key
      + '/export?exportFormat=pdf&format=pdf'
      + '&size=legal'
      + '&portrait=false'
      + '&fitw=false'       // fit to width, false for actual size
      + '&sheetnames=false'
      + '&printtitle=false'
      + '&pagenumbers=false'
      + '&gridlines=false'
      + '&fzr=false'      // do not repeat frozen rows on each page
      + '&gid='
      + index;       //the sheet's Id

      //   Authorisation stuff...
      var token = ScriptApp.getOAuthToken();
      var docurl = UrlFetchApp.fetch(theurl, { headers: { 'Authorization': 'Bearer ' +  token } });
      var pdf = docurl.getBlob().setName(filename).getAs('application/pdf');
      var target = ['EMAIL 1' , 'EMAIL 2'] //example ['m.parzakonis@example.gr','m.parzakonis@example.com']

      // Save the file to folder on Drive
      var fid = 'YOUR FOLDER ID';
      var folder = DriveApp.getFolderById(fid);
      folder.createFile(pdf);
      //   folder.addViewers(target); // add recipients as viewers in the folder. This will sent an email, so comment it out if it bothers you!

      // Send an email with attachment. Taken from https://developers.google.com/apps-script/reference/gmail/gmail-attachment
      var file = DriveApp.getFileById(key);
      MailApp.sendEmail(target, 'Page Speed Insight score report : ' + year + '-' + month, 'Please find attached the latest Page Speed Insight score report, for the ' + month + '\n\nAll previous reports are accessible in this link :' + folder.getUrl() +
      '\nUpdated at ' + file.getLastUpdated() , {
        name: 'Report Emailer Script',
        attachments: [file.getAs(MimeType.PDF)]
        });

      }
```

In order to make it look better we can create a menu item to give access to users of the Sheet to email the report with a click. Creating a menu tab is simple and straightforward ;  we need to declare a name and a `functionName` to attach to the `name`.

``` Javascript
function onOpen() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet();
  var entries = [{
    name : "Query",
    functionName : "pageSpeedInsights"
    },
    {name : "Email report",
    functionName : "spreadsheetToPDF"}
    ];
    sheet.addMenu("Page Speed Insights", entries);
  };
```

Now, the flow `Page Speed Insights > Email report` will sent the email as a pdf to the predefined recipients, along a link to the shared folder with previous reports.

![Page Speed Email](https://dl.dropbox.com/s/10keya4ir0hq38r/page_speed_insight_report_mail.png?dl=0)

# Summary

In this guide we created

  1. a custom formula to query the API
  2. a simple monitoring sheet as a report
  3. scheduled an email to be sent out monthly.

There are many ways that this base script can be extended to facilitate tons of stuff, like detailed monitoring, comparisons between periods, dynamic input of report recipients to name a few. Feel free to fork the code and submit pull requests!
