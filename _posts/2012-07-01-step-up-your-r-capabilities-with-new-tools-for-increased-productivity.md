---
title: Step up your R capabilities with new tools for increased productivity
author: manos_parzakonis
layout: post
permalink: /step-up-your-r-capabilities-with-new-tools-for-increased-productivity/
aktt_notify_twitter:
  - no
categories:
  - statistics
tags:
  - analytics
  - business intelligence
  - qlikview
  - R
---
<p style="text-align: justify;">
  I guess a lot of us actually use many tools to accomplish various things in their everyday life. There is the (not that uncommon) case where you have to build something that others will use in their everyday business life to get insights, information and/or take decisions.
</p>

<p style="text-align: justify;">
  The basic implementation scenario here would be to build an excel workbook where you will feed the data and have a overview sheet, named <em>Dashboard</em>&#8230;If things are on your side you could set-up a connection to a database (an existing one or one you will create for the data in discussion) and pull data from there. You can build powerful and visually elegant things using this approach. A cool resource to generate tears of joy among colleagues is <a href="http://chandoo.org" target="_blank">Chandoo.org</a>.
</p>

<p style="text-align: center;">
  <img class="aligncenter" title="KPI Dashboard in Excel - Snapshot" src="http://i1.wp.com/chandoo.org/img/dashboards/dw/kpi-dashboard-revisited-snapshot.png?resize=620%2C495" alt="KPI Dashboard in Excel - Snapshot" data-recalc-dims="1" />
</p>

<p style="text-align: justify;">
  OK, we all love R. You can make so much things with R that you could set-up a nice view on the issue that is addressed. There are a lot and excellent of graphics initiatives for R (<a title="ggplot2 Home" href="http://had.co.nz/ggplot2/" target="_blank">ggplot2</a> the most appealing to me) to make great dashboards. This question has been asked in many occurrences (example : <a href="http://stackoverflow.com/questions/2196985/information-dashboards-in-r-with-ggplot2">Information Dashboards in R with ggplot2</a> via <a title="stackoverflow" href="http://stackoverflow.com" target="_blank">stackoverflow)</a>
</p>

<p style="text-align: center;">
  <img class="aligncenter" title="ggplot2: Sales Dashboard" src="http://i2.wp.com/learnr.files.wordpress.com/2009/04/sales_dashboard_scen3b.gif?resize=600%2C533" alt="http://i2.wp.com/learnr.files.wordpress.com/2009/04/sales_dashboard_scen3b.gif?resize=600%2C533" data-recalc-dims="1" />
</p>

<p style="text-align: justify;">
  But what about interactive results? This is the main thing that you should validate as the number one target in a dashboard. A static picture is well suited for a report to top level management but mid-level management would expect something that would at least give them the option to explore things. This is the region that BI tools come into game. Now, when your company will acquire such a tool I could bet that they would expect that you spend a<em> looooot</em> of time with this. There are many tools to use and I suggest that you spend time with all of them before turning in a suggestion for buying a licence. You can take a pretty compact list in the <a href="http://www.visualisingdata.com/index.php/2011/03/part-1-the-essential-collection-of-visualisation-resources/" target="_blank">The essential collection of visualisation resources</a> via Visualizing Data blog. I use <a href="http://www.qlikview.com/" target="_blank">QlikView</a> extensively. It has a great community and trust me if you managed to learn R then culprits of QlikView won&#8217;t burden you (they will manage to iritate you, however).
</p>

<p style="text-align: justify;">
  <!--more-->
</p>

<p style="text-align: justify;">
  Unfortunately you will soon realize that building a highly interactive dashboard has limited added value for complex questions, like the ones that predictive analytics would bomb at your inbox. You would not expect I guess to solve a classification problem with clicks I guess. In that case&#8230;Call R! Now, have in mind that the interactive nature of BI tools would enable you to make data management (aka <strong><em>rapidly creating a segment of your data to analyse</em></strong>) and then feed R for analysis. Then the results from R will feedback QlikView and continue with enriched analysis. This is pretty cool, trust me! The following is the approach I am using.
</p>

<div>
  <blockquote>
    <h2>
      Integrating QV with R example kit.zip
    </h2>
    
    <div>
      VERSION 1
    </div>
    
    <div>
      Created on: Apr 25, 2012 11:13 PM by <a id="jive-2340028906824848990109" href="http://community.qlikview.com/people/etk" data-externalid="" data-username="etk" data-avatarid="3926">Elif Tutuk</a> &#8211; Last Modified:  Apr 25, 2012 11:17 PM by <a id="jive-2340028906824849427109" href="http://community.qlikview.com/people/etk" data-externalid="" data-username="etk" data-avatarid="3926">Elif Tutuk</a>
    </div>
    
    <div>
      <div>
        <div id="translate-this">
          <p>
            This paper is designed to provide information explaining how QlikView can be integrated with R. R is an open source programming language and software environment for statistical computing and graphics. The R language is widely used among statisticians for statistical software development and data analysis (<a href="http://www.r-project.org/">http://www.r-project.org/</a>).
          </p>
          
          <p>
            There are a couple of ways to integrate R with other software such as QlikView. This paper focuses on using COM interface and explains the details on the integration. Please note that the integration requires some code development in Visual Basic and a basic understanding of R language. This solution is tested with small data sets and could be suitable for the cases where the business users make selections on the QlikView application and uses the selected data sets for advanced statistics by using R. The goal of this document is to provide some guideline to technical resources on how QlikView R integration can be achieved.
          </p>
        </div>
      </div>
      
      <div>
        <a href="http://community.qlikview.com/servlet/JiveServlet/downloadBody/2975-102-1-3200/Integrating%20QV%20with%20R%20example%20kit.zip" rel="nozoom">Integrating QV with R example kit.zip</a> (9.4 MB)
      </div>
    </div>
  </blockquote>
  
  <p style="text-align: justify;">
    Now, the above example has a setup of sales per zip code. The data are constructed in an order level. The objective of the analysis is to segment the selected zip codes by order frequency, average order size and customer count metrics using R&#8217;s clustering algorithm (K-cluster) . This is accomplished in one click and then the data are available in QlikView for further investigation. As you may have suspected from the quoted description above you will need the <a title="statconn Home" href="http://rcom.univie.ac.at/download.html" target="_blank">statconn</a> package.
  </p>
  
  <div style="text-align: justify;">
    <p>
      Technically you insert in the QlikView Module the R code like this. <em>You will notice that &#8220;R.&#8221; indicates an operation of the R connection. There are a few more R.* that are straightforward to interpret, like R.EvaluateNoReturn, R.GetSymbol, R.Close. </em>Let&#8217;s get a quick walk-through the R module script as far as R is concerned.
    </p>
    
    <pre lang="rsplus">'create a COM object representing R
Set R = CreateObject("StatConnectorSrv.StatConnector")
R.Init "R"</pre>
    
    <p>
      R. Init indicates the start of a R connection
    </p>
    
    <pre lang="rsplus">'Pass data to R from clipboard
R.EvaluateNoReturn "QVData</pre>
    
    <p>
      Calculations that aren&#8217;t returned to the QlikView interface but are instead written to a table for further use
    </p>
    
    <pre lang="rsplus">'Write the data to text file for AUDIT purposes
R.EvaluateNoReturn "setwd('C:\R\')"
R.EvaluateNoReturn "write.table(QVData,'text.txt',col.names = NA)"
'Get R results
RResult=R.GetSymbol("DisplayResult")</pre>
    
    <p>
      Show results and close the connection.
    </p>
    
    <pre lang="rsplus">'Close R connection
R.close</pre>
  </div>
</div>

<!-- MixPanel Start !-->

  
  
<!-- MixPanel End -->