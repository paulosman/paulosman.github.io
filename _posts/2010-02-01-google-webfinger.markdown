--- 
layout: post
title: Google Webfinger
wordpress_id: 267
wordpress_url: http://blog.eval.ca/?p=267
date: 2010-02-01 12:43:16 -05:00
---
<p>This is <a href="http://groups.google.com/group/webfinger/browse_thread/thread/46fe84c1e38ab715/fa625d20f4d963e4">pretty old news</a>, but I missed the original announcement and I think it's pretty interesting.</p>

<p>Google have implemented an alpha of the <a href="http://code.google.com/p/webfinger/">WebFinger protocol</a>.  If you have a Google profile, click on "Edit your profile" and add 'webfingeralpha' as an interest. Congrats, your gmail address is now a WebFinger identifier and will resolve to an XRD file containing information about services you use (if you've included that information in your Google profile).</p>

<p>This is pretty fun to play around with. Given an email-like identifier, such as evalpaul@gmail.com, get the host-meta XRD file for the domain:</p>

<pre lang="bash">
paul@knuth ~ $ curl -i http://gmail.com/.well-known/host-meta
HTTP/1.1 200 OK
Content-Type: application/xrd+xml
Transfer-Encoding: chunked
Date: Mon, 01 Feb 2010 12:36:47 GMT
Expires: Mon, 01 Feb 2010 12:36:47 GMT
Cache-Control: private, max-age=0
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
X-Frame-Options: SAMEORIGIN
Server: GFE/2.0
...
</pre>

<pre lang="xml">
<?xml version='1.0' encoding='UTF-8'?>
<!-- NOTE: this host-meta end-point is a pre-alpha work in progress.   Don't rely on it. -->
<!-- Please follow the list at http://groups.google.com/group/webfinger -->
<XRD xmlns='http://docs.oasis-open.org/ns/xri/xrd-1.0' 
     xmlns:hm='http://host-meta.net/xrd/1.0'>
  <hm:Host xmlns='http://host-meta.net/xrd/1.0'>gmail.com</hm:Host>
  <Link rel='lrdd' 
        template='http://www.google.com/s2/webfinger/?q={uri}'>
    <Title>Resource Descriptor</Title>
  </Link>
</XRD>
</pre>

<p>Now that you have the URI template, get the XRD file for the specific user:</p>

<pre lang="bash">
curl "http://www.google.com/s2/webfinger/?q=acct%3Aevalpaul%40gmail.com"
</pre>

<pre lang="xml">
<?xml version='1.0'?>
<XRD xmlns='http://docs.oasis-open.org/ns/xri/xrd-1.0'>
	<Subject>acct:evalpaul@gmail.com</Subject>
	<Alias>http://www.google.com/profiles/evalpaul</Alias>
	<Link rel='http://portablecontacts.net/spec/1.0' href='http://www-opensocial.googleusercontent.com/api/people/'/>
	<Link rel='http://webfinger.net/rel/profile-page' href='http://www.google.com/profiles/evalpaul' type='text/html'/>
	<Link rel='http://microformats.org/profile/hcard' href='http://www.google.com/profiles/evalpaul' type='text/html'/>
	<Link rel='http://gmpg.org/xfn/11' href='http://www.google.com/profiles/evalpaul' type='text/html'/>
	<Link rel='http://specs.openid.net/auth/2.0/provider' href='http://www.google.com/profiles/evalpaul'/>
	<Link rel='describedby' href='http://www.google.com/profiles/evalpaul' type='text/html'/>
	<Link rel='describedby' href='http://s2.googleusercontent.com/webfinger/?q=evalpaul%40gmail.com&amp;fmt=foaf' type='application/rdf+xml'/>
</XRD>
</pre>

**EDIT:** Google have since rolled out WebFinger support for everyone with a Google Profile. You no longer need to add 'webfingeralpha' to your interests.

