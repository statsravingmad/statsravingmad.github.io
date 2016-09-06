---
title: Back up your WordPress Installation on Google Drive
author: manos_parzakonis
layout: post
permalink: /back-up-your-wordpress-installation-on-google-drive/
excerpt: "Use Google Drive as a backup storage for your Wordpress sites"
categories:
  - Digital Life
tags:
  - backup
  - cloud
  - Drive
  - google
  - wordpress
---
<p style="text-align: justify;">
  So, I got lured into setting a back-up strategy for the present blog, mostly because I cahnge a lot of things at times while trying to learn about wordpress. I could probably do this via a time scheduled job on my hosting provider, but I am not that techy so I searched for relevant plug-ins, and there are a ton (see this <a href="http://wordpress.org/extend/plugins/tags/backup" target="_blank">list</a>). I would like to stress out that I am getting extremely fond of <a href="https://drive.google.com" target="_blank">Google Drive</a> at this moment, so I narrowed the list to those plug-ins that are capable of storing the back-up over my drive.
</p>

<p style="text-align: justify;">
  <em>Confession : I am not sure if this is optimal, though..</em>
</p>

<p style="text-align: justify;">
  From my shortlist I prefered to use <a href="http://david.dw-perspective.org.uk/da/index.php/computer-resources/updraftplus-googledrive-authorisation/?utm_source=statsravingmad.com&utm_medium=post&utm_campaign=back-up-your-wordpress-installation-on-google-drive" target="_blank">UpdraftPlus</a>. It claims to simplifies backups and restoration. You can backup copies of the wordpress databases on the cloud (Amazon S3, Dropbox, Google Drive) to  FTP and email (this would be not of practical use if you have a large folder of upload (>20Mb probably will get cut from the email provider).
</p>

<p style="text-align: justify;">
  The main options are below. You can configure the frequency of the backup (the shortest is 4hours) and the number of backups that will be retained (1 is enough for me). Next check what should be backup&#8217;d. Iback up everything <img src="http://i0.wp.com/www.statsravingmad.com/wp-includes/images/smilies/icon_smile.gif?w=768" alt=":)" class="wp-smiley" data-recalc-dims="1" /> You can set an email to alert you of possible issues (if you don&#8217;t you can always access the log of the process). Last, you can set an encryption password for use when you will need to restore your wordpress database.
</p>

## Configure Backup Contents And Schedule
