---
title: Purge Demographic data in Analytics using Adwords
author: manos_parzakonis
layout: post
permalink: /purge-demographic-data-in-analytics-using-adwords/
excerpt: "A "how-to" find Demographics data in Google Analytics"
bfa_virtual_template:
  - hierarchy
categories:
  - measure
tags:
  - adwords
  - demographic
  - GDN
  - google analytics
  - segmentation
---
<p style="text-align: justify;">
  One of most useful but rarely accomplished to integrate data into your Web Analytics tool is demographic data (in this case the tool is Google Analytics). There are a lot of approach, but it turns out that there is one that could be in front of your eyes from the very beginning&#8230;
</p>

<h3 style="text-align: justify;">
  The data provider
</h3>

<p style="text-align: justify;">
  We will get data from Google Display Network, so we will need a campaign running on the GDN. If you don&#8217;t know how to set-up one it&#8217;s pretty straightforward, just set your ad campaign to &#8220;Display Network only&#8221; or &#8220;Search & Display Networks – All features.&#8221;
</p>

<h3 style="text-align: justify;">
  Add demographic targeting to an ad group
</h3>

<p style="text-align: justify;">
  Now, that you have a campaign set you can add an Audience to run on. There are more than one type of Audiences as you can see in the AdWords <a title="Audiences - Adwords help" href="http://support.google.com/adwords/topic/1713922?hl=en&ref_topic=1713909" target="_blank">help</a>. We are interested in utilising the Demorgaphic option, so we will create an Ad Group to specifically serve us with traffic by age group. I will copy-paste the AdWords instructions below
</p>

  1. Select the ad group to which you&#8217;d like to add demographic categories.
  2. Click the **+Change display targeting** button. Click **Age** to update your demographic targeting by age. <div>
      <div>
      </div>

      <div>
      </div>

      <p>
        <img title="Change display targeting button" alt="Change display targeting button" src="http://storage.googleapis.com/support-kms-prod/SNP_2716691_en_v0" width="799" height="922" data-top="232" data-left="257" data-bottom="266" data-right="439" />
      </p>
    </div>

  3. In the &#8220;Age&#8221; section, click **Edit**.
  4. Select an option below:
      * All ages
      * Specific ages
          * 18-24
          * 25-34
          * 35-44
          * 45-54
          * 55-64
          * 65 or more
          * Unknown
    <div>
      <div>
      </div>

      <div>
      </div>

      <p>
        <img title="Demographic categories for age" alt="Demographic categories for age" src="http://storage.googleapis.com/support-kms-prod/SNP_2716688_en_v0" width="799" height="922" data-top="434" data-left="247" data-bottom="1300" data-right="1315" />
      </p>
    </div>

  5. Click **Done**.
  6. Click **Save** when you&#8217;ve finished updating all of your targeting settings.

<p style="text-align: justify;">
  After this, we have an Ad Group to start with.Now, the tricky part; you need a budget&#8230; ;x
</p>

### Spot data in Google Analytics

<p style="text-align: justify;">
  Where can you find the data in Google Analytics? It is likely that you have noticed that the GDN campaigns sent out to Google Analytics information in the Keyword dimension such as
</p>

<table>
  <tr>
    <td>
      1.
    </td>

    <td>
      (remarketing/content targeting)
    </td>

    <td>
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td>
      2.
    </td>

    <td>
      (content targeting)
    </td>

    <td>
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td>
      3.
    </td>

    <td>
      (not set)
    </td>

    <td>
    </td>

    <td>
    </td>
  </tr>

  <tr>
    <td>
      4.
    </td>

    <td>
      (category matching)
    </td>

    <td>
    </td>
  </tr>
</table>

<p style="text-align: justify;">
  Obviously these are not keywords&#8230;.Now among these you will find keywords that are exactly the Age Groups that you selected to add to your Ad Group targeting. To check that you can use the following Regex filter in the Keyword report of AdWords et voila!
</p>

<pre class="brush: ruby; gutter: true">^(..)to(..)$</pre>

<div id="ID-tab">
  <div id="ID-explorer-table">
    <div id="ID-explorer-table-pieTable">
      <table>
        <tr>
          <th>
          </th>

          <th>
             Keyword
          </th>

          <th>
            Visits
          </th>

          <th>
             Contribution
          </th>
        </tr>

        <tr>
          <td>
            1.
          </td>

          <td>
            25to34
          </td>

          <td>
            2,328
          </td>

          <td>
            36.61%
          </td>

          <td rowspan="10">
            <div>
              <img class="alignright" alt="" src="https://chart.googleapis.com/chart?cht=p&chs=340x240&chp=3.14&chco=058dc7%2C50b432%2Ced561b%2Cedef00&chl=36.61%25%7C27.17%25%7C21.94%25%7C14.28%25&chd=e%3AXbRYOCJI" width="340" height="240" />
            </div>
          </td>
        </tr>

        <tr>
          <td>
            2.
          </td>

          <td>
            18to24
          </td>

          <td>
            1,728
          </td>

          <td>
            27.17%
          </td>
        </tr>

        <tr>
          <td>
            3.
          </td>

          <td>
            35to44
          </td>

          <td>
            1,395
          </td>

          <td>
            21.94%
          </td>
        </tr>

        <tr>
          <td>
            4.
          </td>

          <td>
            45to54
          </td>

          <td>
            908
          </td>

          <td>
            14.28%
          </td>
        </tr>

        <tr>
          <td rowspan="6" colspan="4">
          </td>
        </tr>
      </table>
    </div>

    <div>
      <label> </label>
    </div>

    <h3>
      Segment and slice at will
    </h3>

    <p style="text-align: justify;">
      Obviously the above is only of descriptive value, the true thing starts when you apply this as a <a title="Excellent Analytics Tip#2: Segment Absolutely Everything - Avinash" href="http://www.kaushik.net/avinash/excellent-analytics-tip2-segment-absolutely-everything/" target="_blank">segment</a> (remember segmentation provides insight) or apply a segment to this report. Imagine how insightful this will be when you are testing a new feature that you thing that will change the way that the visitors interact with your website or engaging with your upper conversion funnel steps. I would be happy to have a budget set for traffic of this structure to evaluate my landing pages&#8230;.
    </p>

    <p>
      PS: A similar approach using Facebook PPC data is outlined in the post <a title="Segment Google Analytics data by age and gender - a la data" href="http://aladata.co.uk/segment-google-analytics-data-age-gender/" target="_blank">Segment Google Analytics data by age and gender</a> by <a href="https://plus.google.com/u/0/112461779129810943136">Junta Sekimori</a>
    </p>
  </div>
</div>
