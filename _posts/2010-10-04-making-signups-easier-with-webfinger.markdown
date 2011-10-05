--- 
layout: post
title: Making Signups Easier With WebFinger
wordpress_id: 449
wordpress_url: http://blog.eval.ca/?p=449
date: 2010-10-04 12:17:12 -04:00
---
I've been thinking about signup processes lately and what we can do with <a title="Batucada " href="http://www.drumbeat.org/project/batucada">batucada</a> (the next drumbeat.org) to make it easier, while encouraging users to use open, decentralized identity technologies like <a href="http://openid.net/">OpenID</a>.
<h3>The Problem</h3>
Whenever I come across a new service that I want to create an account on, I dread the idea of creating a new set of credentials and re-entering a bunch of information I have already entered elsewhere. OpenID solves part of this for me, and I'm always happy to see sites supporting it. The problem, however, is that OpenID only solves one piece of the puzzle (authentication) and there are usability problems abound. The latter problem is well known, and people have come up with a bunch of innovative ways of tackling it, including intelligent OpenID selectors.

The core of the problem is that people don't tend to think of themselves as a URL, which is what OpenID uses as a supplied identifier. It's been really hard to get the average web user to think of a URL as part of their identity, and frankly, they're usually pretty hard to remember. There are ways around this, including hosting your own OpenID provider, or using delegation, so that you can choose an easy-to-remember URL, but it's unrealistic to expect non-technical web users to be able to do this.
<h3>The Solution</h3>
Enter <a href="http://code.google.com/p/webfinger/">WebFinger</a>. The idea behind WebFinger is that, given an email-like identifier, an application can discover information about that user (including their OpenID provider). I really like this idea. I think people have an easier time with email addresses than with URLs. A bunch of providers, including AOL, Gmail and Yahoo have implemented WebFinger support, which means that there are a ton of people out there who already have WebFinger identifiers, even if they don't know it (and they shouldn't have to know it).

So what would a WebFinger based signup process look like? The first step would be to capture an email address and find out if it is a valid WebFinger identifier. If it is, the next step would be to find out of the user has an OpenID provider. If their email address is not a WebFinger identifier, the workflow would fall back to a more traditional password based signup form. Imagine clicking on a "Sign up" link and being presented with a form asking for your email address.
<p style="text-align: center;"><a href="http://blog.eval.ca/wp-content/uploads/2010/10/login_step_one.png"><img class="size-medium wp-image-450 aligncenter" title="login_step_one" src="http://blog.eval.ca/wp-content/uploads/2010/10/login_step_one.png" alt="Step One" width="473" height="400" /></a></p>
<p style="text-align: left;">Once submitted, the web application would perform WebFinger based discovery on the identifier in the background. If an OpenID provider is found, the user could be presented with a friendly message asking them if they'd like to use it to sign in to the site.</p>
<p style="text-align: center;"><a href="http://blog.eval.ca/wp-content/uploads/2010/10/mockup.png"><img class="size-medium wp-image-451  aligncenter" title="Step Two" src="http://blog.eval.ca/wp-content/uploads/2010/10/mockup.png" alt="Step Two" /></a></p>
<p style="text-align: left;">If the email address is not a WebFinger identifier, or if the user opts not to use their OpenID, fall back to a more traditional sign up form.</p>
<p style="text-align: left;"></p>
<p style="text-align: center;"><a href="http://blog.eval.ca/wp-content/uploads/2010/10/webfinger_step_two.png"><img class="size-medium wp-image-453 aligncenter" title="webfinger_step_two" src="http://blog.eval.ca/wp-content/uploads/2010/10/webfinger_step_two.png" alt="" /></a></p>
<p style="text-align: left;">These are obviously very rough, but I'm interested to know what people think of this idea. Specifically how the process could be made easier for non-technical users. A few concerns I have (and note that UX / Usability is certainly NOT my specialty, so help is appreciated!):</p>

<ol>
	<li>A service provider shouldn't coerce a user into using an OpenID vs creating a new set of credentials, the user must always have a choice. Unfortunately, this means asking another question, and questions are confusing.</li>
	<li>Presuming a user has an OpenID, but does not have an active session with their provider, they are going to be sent to their provider to log in after Step Two. This could be confusing to users. We're sending them into the OpenID "Chasm of Death". Is there anything that can be done to ease this transition / manage user expectations?</li>
	<li>Once users have signed up using an OpenID, how do we let them know that they should use that OpenID from now on? I suppose we could just prompt for an email address on sign in and perform discovery again. This introduces another step for those who aren't using an OpenID though (step one: input email address, step two on next screen: enter password).</li>
	<li>Users could be confused immediately when we ask for their email address and not a password. This is breaking a familiar pattern, which is always potentially jarring.</li>
	<li>What about users who know what OpenID is, and have an OpenID that they'd like to use, but don't have a WebFinger identifier that refers to their OpenID? Again, more questions to ask, which adds more confusion.</li>
</ol>
Again, I'm certainly not a UX person, so I'd be very curious to hear what people think of this idea and if people have ideas about how to improve the flow.
