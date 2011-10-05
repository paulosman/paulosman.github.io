--- 
layout: post
title: Batucada - Tools & Vision
wordpress_id: 437
wordpress_url: http://blog.eval.ca/?p=437
date: 2010-07-07 14:56:48 -04:00
---
I mentioned in the <a href="http://blog.eval.ca/2010/07/02/batucada-marching-towards-drumbeat-org-1-0/">announcement of batucada</a>, that we are dropping <a title="Drupal" href="http://drupal.org">Drupal</a> for the next phase of development on the Drumbeat website project. We've decided to build Batucada with the Python web framework <a title="Django" href="http://www.djangoproject.com/">django</a>. There are a bunch of reasons why I think django is a good choice, but the primary one is that people are already using it for a number of Mozilla web projects, including AMO and SUMO. Having other people in the organization who are familiar with the framework is helpful, of course, and it means we can reuse work done elsewhere within the webdev team (particularly in fairly generic areas like localization, caching, etc). It also means that our IT department is already familiar with deployment issues.

Django is also simply a great framework. The ability to package up reusable pieces of a project as applications means that we should be able to take advantage of a lot of great work done by folks working on similar web applications, and where we build applications ourselves, we'll be able to release those as separate reusable components. If the work we do can help other people build Django based social web applications that use open web technologies, then we will be further working towards Mozilla's mission of promoting openness, innovation and opportunity on the web.

In order to stay on track, I'm working on a roadmap and a set of core principles that will guide our development of batucada. The roadmap is a work in progress, but I think we've got a good set of principles hashed out:
<ol>
	<li>Social - Batucada will be a social application. People and activities will be at the centre of the experience.</li>
	<li>Intelligent - Batucada should be able to suggest connections based on a users activity and profile information.</li>
	<li>Data Ownership - Users will control who has access to their personal data and will be able to export their data easily.</li>
	<li>Open Web - Batucada will be built using Open Web technologies and will use open standards to integrate with other applications whenever it makes sense to do so.</li>
</ol>
It goes without saying that Batucada will be Open Source as well of course!
