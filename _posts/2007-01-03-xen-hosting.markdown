--- 
layout: post
title: Xen Hosting
wordpress_id: 9
wordpress_url: http://blog.paulosman.com/?p=9
date: 2007-01-03 20:40:56 -05:00
---
<p>
 I recently made the decision to move from a shared hosting account to a dedicated server. I was very happy with my shared hosting provider (<a href="http://www.webfaction.com" title="WebFaction">WebFaction</a>) but it&#8217;s not uncommon for me to have several websites or trac wikis or subversion repositories all going at once. I need these to be accessible to me and my clients at all times and my home DSL connection just isn&#8217;t dependable enough for hosting. I started looking into dedicated hosting packages but decided to save myself about $80 / month and go for a VPS (Virtual Private Server) package. 
</p>

<p>
 When it comes to virtualization, I'm a pretty big fan of <a href="http://www.cl.cam.ac.uk/research/srg/netos/xen/" title="Xen Hypervisor">Xen</a>. If anyone is interested in the subject and hasn&#8217;t already, it&#8217;s really worth checking out. At a really high level, here&#8217;s how it works: basically you have a DOM0 (Domain-0) patched kernel that you run on your physical machine which you allocate a specific amount of RAM to. Once you have your DOM0 set up correctly you can install any number (RAM permitting) of guest hosts running a DOMU (unprivileged domain) patched kernel. You specify how much RAM you want to allocate to each DOMU and there are a number of ways to handle filesystems. The DOM0 (host OS) manages access to hardware and other low-level stuff and basically makes it completely transparent when working within a DOMU. It comes with some handy userland tools to manage guest hosts and there are 3rd party packages like <a href="http://www.enomalism.com" title="Enomalism">enomalism</a> (Yes I just plugged my former employer!) that make it really simple. 
</p>
<p>
So I started shopping around for hosting companies offering VPS packages that used Xen and I found one that looked a little nickle and dime, but were local, had reasonable prices and offered my favourite Linux distribution (<a href="http://www.gentoo.org" title="Gentoo Linux">Gentoo!</a>). I figured I&#8217;d give them a shot. One week and 14 hours of downtime later I canceled my account, got a refund and signed up for an account with <a href="http://rimuhosting.com" title="RimuHosting">RimuHosting</a>. Now these people know how to do support! Within minutes of signing up I got an email with a link to setup my PayPal subscription. I setup my subscription and less than an hour later had my account details. First things first, I tried to connect to my host via SSH and got a timeout... good opportunity to try out their support! I emailed their tech support and we were able to figure out that the problem was a lack of a reverse DNS entry on the VPS' IP address. I didn&#8217;t know that Mac OS X won&#8217;t open an SSH connection in these circumstances. Okay, easy to solve... there&#8217;s a handy utility in their control panel to do just that. Once that was working I was up and running and started to set up Apache, Postfix and all the other wonderful software I use. 
</p>
<p>
Now I&#8217;m happily hosting several websites (each with MySQL or PostgreSQL databases), Trac wikis, SVN repositories, I&#8217;m running AWStats, hosting my mail, and generally loving the VPS life, all for at least a 5th of what a dedicated package would cost!
</p>
