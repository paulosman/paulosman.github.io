--- 
layout: post
title: Shipped New Drumbeat.org
wordpress_id: 511
wordpress_url: http://blog.eval.ca/?p=511
date: 2011-02-14 09:17:19 -05:00
---
A week ago we shipped the new version of <a href="http://drumbeat.org/">drumbeat.org</a>. This was a ground up rewrite and represents a significant change in the direction of the site. Before I go into too many details, or start talking about what's next, I want to thank everyone who has committed code to the project so far:
<ul>
	<li><a title="Jaco Joubert" href="http://www.designisreason.com/">Jaco Joubert</a> - Design</li>
	<li><a title="Ned Schwartz" href="http://theinterned.net/">Ned Schwartz</a> - Front end hacking</li>
	<li><a title="Brian Brennan" href="http://words.brianloves.it/">Brian Brennan</a> - Initial markup and css cutup, front end rescue operative</li>
	<li><a title="Cillian O'Ruanaidh" href="http://cilliano.com/">Cillian O'Ruanaidh</a> - Tester, Django hacker</li>
	<li><a title="Guillermo Movia" href="http://identi.ca/deimidis">Guillermo Movia</a> - Markup and css for those welcome and error messages.</li>
</ul>
<h3>So, What's New?</h3>
When we decided to rewrite drumbeat.org in Django, we also decided to revisit our goals for the site. The old site tried to do a lot of things and we'd prefer to do fewer things and do them well. We decided that drumbeat.org is a social web application that should allow a user to link to content all over the web and have their activities (blog posts, wiki edits, github commits) aggregated in one place. With that in mind, we built the new drumbeat.org. There's a lot of refining to do, but that's the essence of the new site: find people and projects to follow, create projects, and aggregate activity relevant to your project.
<h3>What about my blog posts / pictures / text?</h3>
Most data, including user accounts and projects was automatically migrated over. You should even be able to log in with your old password. Some things however, like blog posts, don't exist on the new site. The old site will be available at old.drumbeat.org until March 15th, 2011. You'll be able to salvage whatever content you may have on the site before then.
<h3>What now?</h3>
Getting to a point where we were comfortable shipping was the first step. We'll now be shipping updates as often as we can. See the (constantly in progress) <a href="https://wiki.mozilla.org/Drumbeat/Batucada/Roadmap">Roadmap</a> for an idea of what we'd like to do over the next couple of quarters. We're also <a href="http://hire.jobvite.com/CompanyJobs/Careers.aspx?c=qpX9Vfwa&amp;cs=9Kt9Vfw1&amp;page=Job%20Description&amp;j=ouAzVfwi">hiring a full-time front-end dev</a> which will double the number of full-time technical staff we have dedicated to drumbeat.org. As such, I'm planning to start having weekly planning calls that will be open to the community. One week will be dedicated to planning an iteration (deciding on what bugs / features to tackle for a specific iteration) and the next one will serve as a status check-in. 2011 is going to be an exciting time for drumbeat.org. I'm so glad to be out of the building phase and into the shipping phase.
<h3>This is really great, but what it's really missing is _____.</h3>
I love feedback and I love ideas about what would be useful for drumbeat.org. If you have some feature that you feel is absolutely essential, there are a few things you can do:
<ol>
	<li>Check the <a href="https://wiki.mozilla.org/Drumbeat/Batucada/Roadmap">Roadmap</a>. If it's already there, we're planning to add it.</li>
	<li>Check <a href="https://bugzilla.mozilla.org/buglist.cgi?product=Websites&amp;component=www.drumbeat.org&amp;resolution=---">Bugzilla</a>. It's possible someone has already thought of this and decided to file a bug on it.</li>
	<li>Bring it up on the <a href="http://www.mozilla.org/about/forums/#drumbeat-website">mailing list.</a> This is a good chance to see if other people share your passion for the feature you're thinking of.</li>
	<li><a href="https://bugzilla.mozilla.org/enter_bug.cgi?product=Websites&amp;component=www.drumbeat.org">File a bug</a>. If you feel like it's worth lobbying for, file a bug requesting the feature.</li>
</ol>
It's important to note that simply requesting a feature will not make it so. It's entirely possible that a particular feature does not align with the goals of the project and will therefore not be implemented.
<h3>I found a bug</h3>
Fantastic. This is extremely helpful. If you feel you've found a bug, the process is similar to above, only shorter!
<ol>
	<li>Check <a href="https://bugzilla.mozilla.org/buglist.cgi?product=Websites&amp;component=www.drumbeat.org&amp;resolution=---">Bugzilla</a>. It's possible someone has already filed a bug about the issue you've encountered. If they have, feel free to comment on the bug.</li>
	<li><a href="https://bugzilla.mozilla.org/enter_bug.cgi?product=Websites&amp;component=www.drumbeat.org">File a bug</a>. Include a description of what you were doing, as well as steps to reproduce (if possible).</li>
</ol>
That's it! Filing bugs is a great way to help the project.
