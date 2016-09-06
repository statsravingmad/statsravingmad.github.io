---
title: Self-hosted Google Plus Profile URL (trick)
author: manos_parzakonis
layout: post
permalink: /self-hosted-google-plus-profile-url-trick/
aktt_notify_twitter:
  - no
  - no
categories:
  - Digital Life
tags:
  - friendly URL
  - google plus
  - redirection
  - wordpress
---
<p style="text-align: justify;">
  It is cumbersome to have a profile link like this
</p>

<p style="text-align: justify;">
  <span style="color: #333333;"><a title="Manos' Google Plus profile" href="https://plus.google.com/u/0/113064680627051167918" target="_blank">https://plus.google.com/u/0/113064680627051167918</a></span>
</p>

<p style="text-align: justify;">
  This is hardly memorable and in case of a brand this can be a drawback. There are shorteners like <a href="http://gplus.to" target="_blank">gplus.to</a> or <a href="http://www.plusya.com" target="_blank">plusya.com</a> but there are not really my style&#8230;What I would idealy want to have as a brand or a person would be the URL to be in the fashion of [<span style="color: #3366ff;">mydomain.com</span>]/googleplus. How can I achieve this? The good ol&#8217; redirection trick can do this! I run on WordPress (revealed <a title="Reset your password on a WordPress installation" href="http://www.statsravingmad.com/blog/digital-life/reset-your-password-on-a-wordpress-installation/" target="_blank">here</a>) so I can use one of the Redirection plug-ins to do the trick (for example this <a title="Redirection Plug-in for WordPress" href="http://wordpress.org/extend/plugins/redirection/" target="_blank">one</a>). In my case I want to have the <a title="Manos' Google Plus profile" href="http://parzakonis.info/+" target="_blank">parzakonis.info/+</a> as my friendly URL so I set the page of my domain to redirect on the (/+)
</p>

<p style="text-align: justify;">
  Source URL:
</p>

<p style="text-align: justify;">
  <input id="old" type="text" name="source" />
</p>

<p style="text-align: justify;">
  and then the target of the redirection to (that would be <a title="Manos' Google Plus profile" href="https://plus.google.com/u/0/113064680627051167918" target="_blank">https://plus.google.com/u/0/113064680627051167918</a>)
</p>

<p style="text-align: justify;">
  Target URL:
</p>

<p style="text-align: justify;">
  <input type="text" name="target" />
</p>

<p style="text-align: justify;">
  Check the 301 Redirection box to redirect in permanently and your are done! I can use the <a title="Manos' Google Plus profile" href="http://parzakonis.info/+" target="_blank">parzakonis.info/+</a> around the web to link to my profile!
</p>