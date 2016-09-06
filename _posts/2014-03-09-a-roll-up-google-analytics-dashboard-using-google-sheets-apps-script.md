---
title: 'A Roll-up GA dashboard using Google Sheets &#038; Apps Script'
author: manos_parzakonis
layout: post
permalink: /a-roll-up-google-analytics-dashboard-using-google-sheets-apps-script/
excerpt: "Create a dashboard for your various accounts!"
categories:
  - Digital Life
  - measure
tags:
  - apps script
  - dashboard
  - Drive
  - google
  - google analytics
  - magic script
  - tracking
  - web analytics
---
<p style="text-align: justify;">
  If you manage a lot of web properties then you surely have run into the problem of replicating the same report across all of the web properties. While, having the ability to create an aggregation of web properties is the most convenient (a <em>roll-up</em> account) there are cases where you cannot actually implement it or the implementation of such a solution will either require significant cut back on data points sent to the roll-up as Google Analytics comes with limitations to quota usage. It always advisable to check your current utilization, you can do it using this <a title="Hitcheck" href="http://hitcheck.analyticspros.com/" target="_blank">tool</a>. As always, reality is not ideal so a roll-up account might not be available. In this case our fallback data retrieval mechanism is the Google Analytics API. We have described ways to access the API using R in two posts : <a title="Google Analytics + R = FUN!" href="http://www.statsravingmad.com/measure/google-analytics-r-fun/" target="_blank">Google Analytics + R = FUN!</a> & <a href="http://www.statsravingmad.com/statistics/basic-ab-testing-plots-and-stats-with-r/" rel="bookmark">Basic A/B Testing plots and stats with R</a>. In the context of our post we need to create a simple dashboard that tracks the conversion rate of our handful of web properties across channels, so this is something that we would pursue using the Google Apps Script under the notorious name <em>Magic Script. </em>
</p>

<h2 style="text-align: justify;">
  Getting the Magic Script
</h2>

<p style="text-align: justify;">
  Open the Script Editor in a Google Spreadsheet clear the code appeared and paste the Magic Script code found in this <a href="https://gist.githubusercontent.com/IronistM/9411651/raw/4f1872333b4383c6f59ee1981018a99362bfc512/magic_script_modified.js" target="_blank">gist</a>. <em>Note : Use this gist because it contains some modifications compared to the initial Magic Script from the GA team.</em>
</p>

<p style="text-align: justify;">
  If you haven&#8217;t used the Google APIs in apps scripts before go through the following steps to enable the Google Analytics API:
</p>

<ul style="text-align: justify;">
  <li>
    In the script editor go to: Resources > Use Google APIs
  </li>
  <li>
    Turn &#8216;Google Analytics API&#8217; to <code>ON</code>
  </li>
  <li>
    Click the link to <code>Google APIs Console</code>
  </li>
  <li>
    Turn &#8216;Google Analytics API&#8217; to <code>ON</code>
  </li>
  <li>
    Accept the terms of service
  </li>
  <li>
    Close the &#8216;Google APIs Console&#8217; window
  </li>
  <li>
    Click <code>OK</code> in the script editor window and close that window
  </li>
  <li>
    Back in the spreadsheet, the &#8216;Google Analytics&#8217; menu should now be working. (You may need to re-authenticate.)
  </li>
</ul>

<p style="text-align: justify;">
  Now, a new tab should appear next to &#8216;Help&#8217; names Google Analytics. If you click it you will see that you can create not only basic reports configuration but Multi-channel funnel reports as well.
</p>

## Define a report

<p style="text-align: justify;">
  We will track conversion rate for the last 30 days. This means that we will not store data but we will only use the fact that the API can query based on relative dates. Let&#8217;s create a core report
</p>

<table dir="ltr" border="1" cellspacing="0" cellpadding="0">
  <colgroup> <col width="100" /> <col width="450" /></colgroup> <tr>
    <td data-sheets-value="[null,2,&quot;query1&quot;]">
      query1
    </td>

    <td data-sheets-value="[null,2,&quot;value1&quot;]">
      value1
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;type&quot;]">
      type
    </td>

    <td data-sheets-value="[null,2,&quot;core&quot;]">
      core
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;ids&quot;]">
      ids
    </td>

    <td data-sheets-value="[null,2,&quot;ga:186644&quot;]">
      ga:XXXXX
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;start-date&quot;]">
      start-date
    </td>

    <td data-sheets-numberformat="[null,5]">
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;end-date&quot;]">
      end-date
    </td>

    <td data-sheets-numberformat="[null,5]">
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;last-n-days&quot;]">
      last-n-days
    </td>

    <td data-sheets-value="[null,3,null,30]" data-sheets-numberformat="[null,0]" data-sheets-formula="=R[-4]C[-2]">
      30
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;metrics&quot;]">
      metrics
    </td>

    <td data-sheets-value="[null,2,&quot;ga:Transactions,ga:visits&quot;]" data-sheets-formula="=R[-4]C[-2]&&quot;,&quot;&R[-3]C[-2]">
      ga:Transactions,ga:visits
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;dimensions&quot;]">
      dimensions
    </td>

    <td data-sheets-value="[null,2,&quot;ga:date&quot;]" data-sheets-numberformat="[null,0]" data-sheets-formula="=R[2]C[-2]">
      ga:date
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;sort&quot;]">
      sort
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;filters&quot;]">
      filters
    </td>

    <td data-sheets-value="[null,2,&quot;ga:eventLabel=~Amadeus&quot;]" data-sheets-numberformat="[null,0]" data-sheets-formula="=R[63]C[0]">
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;segment&quot;]">
      segment
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;start-index&quot;]">
      start-index
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;max-results&quot;]">
      max-results
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,2,&quot;sheet-name&quot;]">
      sheet-name
    </td>

    <td data-sheets-value="[null,2,&quot;pd_data&quot;]">
      Website_A_data
    </td>
  </tr>
</table>

<p style="text-align: justify;">
  <span style="line-height: 1.5em;">The report you will define here will be replicated for all web properties you have. To replicate the report you have to simply drag the first row until the aut0 increment of queryX,valueX reaches the number of web properties. Also, name the target sheet accordingly to reflect the web property. Then you need to create a intermediate sheet to aggregate the data (as the data will be refreshed on the target sheet hence every run will clear all contents of the sheet) plus in the future you might need to add more dimensions to the query that will result to more columns added). Essentially, in a new sheet we aggregate data from the queries&#8217; fed sheets, like below. <em> </em></span>
</p>

<p style="text-align: justify;">
  <em>Tip : Create the first formula and then select Show all formulas from the View menu. Then simply Find & replace the sheet name in the respective web property column and you are done!</em>
</p>

<table dir="ltr" cellspacing="0" cellpadding="0">
  <colgroup> <col width="128" /> <col width="127" /> <col width="101" /> <col width="98" /> <col width="120" /> <col width="121" /> <col width="109" /> <col width="109" /></colgroup> <tr>
    <td data-sheets-value="[null,2,&quot;ga:date&quot;]" data-sheets-numberformat="[null,0]" data-sheets-formula="=UNIQUE(fg_data!R12C1:R1500C1)">
      =UNIQUE(website_A_data!$A$12:$A$150)
    </td>

    <td data-sheets-value="[null,2,&quot;fantasticgreece&quot;]">
      website_A
    </td>

    <td data-sheets-value="[null,2,&quot;airtickets24&quot;]">
      website_B
    </td>

    <td data-sheets-value="[null,2,&quot;trip.ru&quot;]">
    </td>

    <td data-sheets-value="[null,2,&quot;avion.ro&quot;]">
    </td>

    <td data-sheets-value="[null,2,&quot;pamediakopes&quot;]">
    </td>

    <td data-sheets-value="[null,2,&quot;trip.bg&quot;]">
      website_N
    </td>

    <td data-sheets-value="[null,2,&quot;Total&quot;]">
      Total
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,3,null,41670]" data-sheets-numberformat="[null,5]" data-sheets-ischild="">
      1/31/2014
    </td>

    <td data-sheets-value="[null,3,null,2]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(fg_data!R[11]C[0])">
      =sum(website_A_data!B13)
    </td>

    <td data-sheets-value="[null,3,null,316]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(at24_data!R[11]C[-1])">
      =sum(website_B_data!B13)
    </td>

    <td data-sheets-value="[null,3,null,567]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.ru_data!R[11]C[-2])">
    </td>

    <td data-sheets-value="[null,3,null,128]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(avion.ro_data!R[11]C[-3])">
       &#8230;
    </td>

    <td data-sheets-value="[null,3,null,384]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(pd_data!R[11]C[-4])">
    </td>

    <td data-sheets-value="[null,3,null,71]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.bg_data!R[11]C[-5])">
      =sum(website_N_data!B13)
    </td>

    <td data-sheets-value="[null,3,null,1468]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(R[0]C[-6]:R[0]C[-1])">
      =sum(B2:G2)
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,3,null,41671]" data-sheets-numberformat="[null,5]" data-sheets-ischild="">
      2/1/2014
    </td>

    <td data-sheets-value="[null,3,null,0]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(fg_data!R[11]C[0])">
      =sum(website_A_data!B14)
    </td>

    <td data-sheets-value="[null,3,null,311]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(at24_data!R[11]C[-1])">
      =sum(website_B_data!B14)
    </td>

    <td data-sheets-value="[null,3,null,590]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.ru_data!R[11]C[-2])">
    </td>

    <td data-sheets-value="[null,3,null,54]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(avion.ro_data!R[11]C[-3])">
    </td>

    <td data-sheets-value="[null,3,null,285]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(pd_data!R[11]C[-4])">
    </td>

    <td data-sheets-value="[null,3,null,24]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.bg_data!R[11]C[-5])">
      =sum(website_N_data!B14)
    </td>

    <td data-sheets-value="[null,3,null,1264]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(R[0]C[-6]:R[0]C[-1])">
      =sum(B3:G3)
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,3,null,41672]" data-sheets-numberformat="[null,5]" data-sheets-ischild="">
      2/2/2014
    </td>

    <td data-sheets-value="[null,3,null,0]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(fg_data!R[11]C[0])">
      =sum(website_A_data!B15)
    </td>

    <td data-sheets-value="[null,3,null,335]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(at24_data!R[11]C[-1])">
      =sum(website_B_data!B15)
    </td>

    <td data-sheets-value="[null,3,null,457]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.ru_data!R[11]C[-2])">
    </td>

    <td data-sheets-value="[null,3,null,57]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(avion.ro_data!R[11]C[-3])">
    </td>

    <td data-sheets-value="[null,3,null,278]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(pd_data!R[11]C[-4])">
    </td>

    <td data-sheets-value="[null,3,null,31]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.bg_data!R[11]C[-5])">
      =sum(website_N_data!B15)
    </td>

    <td data-sheets-value="[null,3,null,1158]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(R[0]C[-6]:R[0]C[-1])">
      =sum(B4:G4)
    </td>
  </tr>

  <tr>
    <td data-sheets-value="[null,3,null,41673]" data-sheets-numberformat="[null,5]" data-sheets-ischild="">
      2/3/2014
    </td>

    <td data-sheets-value="[null,3,null,4]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(fg_data!R[11]C[0])">
      =sum(website_A_data!B16)
    </td>

    <td data-sheets-value="[null,3,null,431]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(at24_data!R[11]C[-1])">
      =sum(website_B_data!B16)
    </td>

    <td data-sheets-value="[null,3,null,829]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.ru_data!R[11]C[-2])">
    </td>

    <td data-sheets-value="[null,3,null,93]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(avion.ro_data!R[11]C[-3])">
    </td>

    <td data-sheets-value="[null,3,null,413]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(pd_data!R[11]C[-4])">
    </td>

    <td data-sheets-value="[null,3,null,66]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(trip.bg_data!R[11]C[-5])">
      =sum(website_N_data!B16)
    </td>

    <td data-sheets-value="[null,3,null,1836]" data-sheets-numberformat="[null,2,&quot;#,##0&quot;,1]" data-sheets-formula="=sum(R[0]C[-6]:R[0]C[-1])">
      =sum(B5:G5)
    </td>
  </tr>
</table>

<p style="text-align: justify;">
  Create blocks of the above defined by the metrics you have defined in the report. this means you will make another table as above with source data the <span style="color: #808080;">sheet-name</span>!C13 which will give you a table with the visits. Then creating row totals will give us daily totals for all web properties reported and column totals will  give period wide totals for each web property. Last a sum of all cells in a block will give the total visits/conversions/dimensionX for the period summing over web properties. The division of totals gives period wide conversion rate for all wep properties and a cell by cell division provides the conversion rate for a specific web property for a day.
</p>

## How to fight Execution Errors

Google spreadsheets have a time limit of 5 minutes when running scripts. So, if you have a heavy on queries spreadsheet you might get errors like that. The solution to this is to break down queries to more spreadsheets and then import data into your dashboard (the master spreadsheet) using the <a title="importrange" href="https://support.google.com/drive/answer/3093340?hl=en" target="_blank">importrange()</a> function which is magical by all means! However, in our case here if an error occurs (lets assume that it will occur in the last query) you can let viewers of the dashboard know using a message in cell using the next if() statement.

<pre class="brush: sql; gutter: false">=if(now()-B2&gt;0.019,"Beware : Execution Error in the last run!","")</pre>

<h2 style="text-align: justify;">
  Dashboard without sparklines?
</h2>

<p style="text-align: justify;">
  No sir! In case you didn&#8217;t know Google Spreadsheets have a <a href="https://support.google.com/drive/answer/3093289" target="_blank">sparkline() </a>function to use for the dashboards you are designing which are a great source of eye-picking information for spotting ups and downs in the time trends of your metrics and KPIs. Make sure that you won&#8217;t exaggerate like me with the use of sparklines&#8230;
</p>

<p style="text-align: justify;">
  <strong>Sparkline of Type I : Line</strong>
</p>

<p style="text-align: justify;">
  Line will show you time evolution, for your conversion rates, traffic, qualified visits etc. Look for peaks and lows or relative flat linings
</p>

<p style="text-align: justify;">
  <strong>Sparkline of Type II : Bar</strong>
</p>

<p style="text-align: justify;">
  Really nice to show you relative performance, say today vs yesterday (or vs this day last week). To use this the formula is simply
</p>

<pre class="brush: sql; gutter: false">=SPARKLINE(B6:C6,{"charttype","bar"})</pre>

<h2 style="text-align: justify;">
  In the end
</h2>

<p style="text-align: justify;">
  The following is a sample dashboard with a few runs on my personal site and some other blogs I have access to. It&#8217;s not really e-commerce rate but rather goal conversion rate for a contact form submission. Take a look at this and get some ideas on how to create more for your personal dashboard.
</p>

<h2 style="text-align: left;">
  There is no end, if there is no tracking code added&#8230;
</h2>

<p style="text-align: justify;">
  The Magic Script you got above is modified and enables you to track the usage of the Spreadsheet (using pageviews) and the execution of the getData function with event tracking. simply open the Script Editor and replace your UA in the script. I suggest having a separate Google Apps web property and use a content grouping per Google App (spreadsheets, docs, forms etc) to track utilisation of your drive creations. This is really exciting to see how (and if!) people are actually using the docs you shared like the screenshot below.
</p>

Happy dashboarding!

&nbsp;
