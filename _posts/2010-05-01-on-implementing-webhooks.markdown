--- 
layout: post
title: On Implementing WebHooks
wordpress_id: 337
wordpress_url: http://blog.eval.ca/?p=337
date: 2010-05-01 22:25:38 -04:00
---
<p>I wrote about our experiences implementing WebHooks at FreshBooks yesterday. You can read all the gory details <a href="http://developers.freshbooks.com/blog/view/on_implementing_webhooks/">here</a>. Quick summary is: use a Message Queue, send HTTP requests asynchronously, have a retry queue, verify ownership of URIs.</p>
