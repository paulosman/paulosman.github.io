---
layout: post
title: Production Driven Development
---

Throughout my career, I've developed some
opinions. Okay, that's an understatement. We work in an industry that is full
of opinions and I have many. Some of my opinions came and went with new
information. I try to challenge strongly held beliefs I have and have changed
my opinion frequently. Others, however, have worn particularly deep ruts,
reinforced by years of experience. I tried to think about what my most strongly
held opinions had in common, and it occurred to me that they all centered on
the idea that **code in production is the only code that matters**. Staging doesn't
matter, code on your laptop doesn't matter, QA doesn't matter, literally,
everything else is debt (some of which might be necessary, some of which can be
done away with). This perspective probably comes from years sitting in between
what is classically called operations and product development. I strongly
believe that teams should optimize for getting code to production as quickly as
possible, as well as responding to issues in production.

Apparently this idea, and a lot of the practices it implies, can be
counter-intuitive or controversial, so I want to dive into them a little
further. What follows is a set of practices and principles I believe are true,
considering my underlying belief that code working in production is the only
code that matters.

## 1. Engineers should operate their own code.

**Engineers are the subject matter experts for the code they write and should be
responsible for operating it in production**. In this context, "operating" means
deploying, instrumenting, and monitoring code as well as helping to resolve
incidents related to or impacting that code. The responsibility of operating
code aligns incentives - it encourages engineers to write code that is
observable and easy to debug, and connects them to what customers really care
about. It encourages them to be curious about how their code is performing in
production. Importantly, engineers should be on-call for their code - being
on-call creates a positive feedback loop and makes it easier to know if their
efforts in writing production-ready code are paying off. I've heard people
complain about the prospect of being on-call, so I'll just ask this: if you're
not on-call for your code, who is?

If you're not currently on-call for your own code but want to be, and can help
influence this decision, there are some things you can do. Set up PagerDuty (or
similar) schedules for each group of engineers responsible for specific
services or parts of your code. A good schedule has 6–8 engineers. There are
plenty of variations, but a typical template is to have one-week rotations,
where you'll be on-call for secondary for a week and then primary for a week.
Configuring alerts is a separate topic, which probably deserves it's own blog
post entirely, but focus on things that impact your customers (see:
Symptom-based alerting) and remember that you're ultimately responsible for how
you respond to alerts, which means you can change them.

There are two talks I'd recommend watching that touch on the topic of
configuring alerts: Liz Fong-Jones talks about SLOs in [Cultivating Production Excellence](https://www.youtube.com/watch?v=MT4jbUzEVL0) and Aditya Mukerjee does a great job talking about techniques for
managing alerts in [Warning: This Talk Contains Content Known to the State of California to Reduce Alert Fatigue](https://vimeo.com/274820572).

## 2. Buy Almost Always Beats Build

**If you can avoid building something, you should.** Code is the most expensive
way to solve a problem that isn't addressing a core area of your business. For
most small to mid-sized companies, there are open source or better yet, hosted
solutions that solve a wide range of common problems. I mean things like git
repository hosting (Github, Gitlab, Bitbucket, etc), observability tooling
(Honeycomb, Lightstep, etc), managed databases (Amazon RDS, Confluent Kafka,
etc), alerting (PagerDuty, OpsGenie, etc) and a whole host of other commodity
technologies. This even applies to your own infrastructure - if you can help
it, don't roll your own Kubernetes clusters (side note: do you even need to use
Kubernetes?), don't roll your own load balancers if you can use Amazon ELB or
ALBs.

Unfortunately, [NIH](https://en.wikipedia.org/wiki/Not_invented_here) syndrome is very real and some companies get burned badly by
this. I've seen teams light time and money on fire reinventing components when
better, more battle-tested alternatives exist in the market. Those same teams
almost always end up spending years contending with the resulting technical
debt. If you're on such a team and have the will and ability to impact change,
start rolling back these decisions one by one. Migrate your databases to a
managed provider, migrate your feature flagging system to a SaaS tool (i.e.
LaunchDarkly). Keep going until the only software you maintain yourselves is
the software that delivers value to your customers. You'll be much, much better
off for it.

## 3. Make Deploys Easy

Deploying should be a frequent and unexciting activity. **Engineers should be
able to deploy with minimal manual steps and it should be easy to see if the
deploy is successful** (this requires instrumenting your code for observability,
which - tada - is covered above), and it should be easy to roll back a deploy
in the event that something doesn't go well. Deploying frequently implies that
deploys are smaller, and smaller deploys are generally easier, faster and
safer.

Many teams implement periods of time where deploys are forbidden - these can be
referred to as code freezes, or deploy policies like "Don't deploy on Fridays".
Having such blackout periods can lead to a pile-up of changes, which increases
the overall risk of something going very wrong.

If you're on a team that fears deploys, dedicate a percentage of your
engineering time to improvements in your deployment pipeline until the fear is
gone. On a recent team I worked with, we were able to improve deploy times from
3 hours to 30 minutes, which drastically improved the teams' confidence in the
deploy process. A natural side effect of this was that engineers started
deploying much more frequently instead of waiting for changes to pile up enough
to warrant a "release" (which was synonymous with a deploy).

The book
[Accelerate](https://www.amazon.com/Accelerate-Software-Performing-Technology-Organizations/dp/1942788339)
has been getting a lot of attention. If you haven't read it, I'd recommend it.
The team behind it also publishes the [State of DevOps](https://cloud.google.com/devops/)
reports, which are full of well-researched information about what various
companies in the industry are doing. It's not a coincidence that two of the
four key metrics that the book focuses on are directly related to this (Deploy
Frequency, Change Lead Time). Shipping really is [your company's heartbeat](https://www.heavybit.com/library/podcasts/o11ycast/ep-12-speed-of-deployment-with-rich-archbold-of-intercom/).

## 4. Trust the People Closest to the Knives

**The people who work with a system are the ones who understand it best.** This
applies to any part of the socio-technical systems within which we all work. In
the case of software systems, the engineers who deploy every day and are
on-call for critical services understand the level of risk they operate in. A
sad trend is that managers tend to overestimate their teams' progress on
certain transitions - i.e. cloud-native, DevOps, etc. The higher up the
management chain, the larger this overestimation tends to be. Engineers who
deploy and get paged when things break know where the bodies are buried and
they know what needs the most work. They should, therefore, be the primary
stakeholders responsible for prioritizing technical work.

Another manifestation of this principle applies to platform or services teams.
If you're responsible for building some shared component that's used within
your organization (i.e. a messaging system, ci/cd infrastructure, shared
libraries or services) there's an uncomfortable truth lurking for you: the
people who use your work know more about it than you do in many cases. They
understand implicitly how it serves customers and they know what contortions or
hoops they have to jump through in order to get it to work. Listen to them for
clues on how to improve the UX of your services and tools.

## 5. QA Gates Make Quality Worse

Many teams have a manual QA step that gets performed before deploys. The idea,
I guess, is to have someone run automated or manual tests to verify that a set
of changes are ready to be released. This sounds like a comforting
idea - having a human being (or team of human beings) "verify" a release before
it goes out - **but it falls victim to a number of false assumptions and creates
some misalignments that do more harm than good.**

First of all, if there's manual work that needs to be done before a deploy can
go out, that creates a bottleneck - if you're making deploys easy, and
deploying small changes frequently, no QA team is going to be able to keep up
testing every deploy, and will inevitably block teams from deploying. That's no
good. If you have manual tests, automate them and build them into your CI
pipeline (if they actually do deliver value).

Secondly, the teams doing QA often lack context and are under time pressure.
They may end up testing "effects" instead of "intents". For example, I've seen
QA teams burn time testing that when something happens in a UI, something
related happens in a database. What happens when an engineer refactors that UI
component and changes the underlying data model? The functionality works, but
the test breaks. Because two teams are involved, this takes coordination and
time to fix. Similarly, I've seen QA teams block deploys because of failing
tests when caching was introduced at the CDN layer - a TTL of 5 seconds on an
activity feed may not ever be noticed by a user but it might break QA tests
causing unnecessary conflicts between product and QA engineers.

Luckily, solving this one is easy. Instead of having a dedicated QA team work
on creating manual and automated test cases that run in a fictitious QA
environment, reassign that team to work on continuous testing in production.
Instead of being a gate for deploys, a QA team could continuously verify that
production is working as expected. QA teams are also well situated to lead
Chaos Engineering initiatives, where faults are intentionally injected in
production. QA engineers could also work on making the CI/CD pipeline more
reliable, so that deploys are no longer a nightmare.

## 6. Boring Technology is Great.

With thanks to [Dan McKinley](https://mcfunley.com/choose-boring-technology),
always **strive for boring tech when possible**. Systems are inherently
unpredictable, and you want a wide area of expertise to fall back on when shit
goes sideways. There are also routine operations that you'll have to do
(deploys, database migrations, etc) and it's Very Nice to have widely used and
tested tooling for this stuff. I think of databases most often when I think
about this belief. MySQL is a database with many, many quirks, but it is so
widely used, that you should still just use it most of the time.

Very few organizations have the bandwidth to debug unique problems. You really
don't want unique problems, especially when performing routine
operations - i.e. storing bytes on disk, choosing a new leader in a cluster,
garbage collecting objects, querying time-series data, etc. Having unique
problems will kill a small to medium size team. It will sap you of your
creative energy, which is better used creating value for customers who want to
pay you monies for your software. Use your innovation tokens wisely!

Using boring technology means you can lean on a large community of users. Shit
on it all you want, but there are very few PHP issues that someone else hasn't
already encountered. Nowadays, the same is probably true for sufficiently
widely used versions of Ruby on Rails. I often say that I like to be in the 3rd
cohort of technology adoption. The 1st cohort is the bleeding edge
organization. The 2nd cohort is the people who feel like they can take some
risks. Let those two groups go before you, run into all the big problems, and
then you can go, benefiting from all of their hard-won experience.

## 7. Simple Always Wins

I don't have much to say about this, but we're all writing YAML and JSON
instead of XML and we're all using HTTP instead of CORBA, RMI, DCOM, XPCOM,
etc. Right? In that same spirit, I'd rather debug problems in a LAMP stack than
a Microservices architecture any day.

Quick sidebar on Microservices: as with so many trends in tech, they are often
sold as a panacea. Let me be clear: **Microservices, designed well, solve some
specific problems and as with most solutions to complex problems, involve a
number of trade-offs**. If you are going in this direction, I do have [opinions](https://www.amazon.com/Microservices-Development-Cookbook-independently-deployable/dp/1788479505) on
how you should do it, but I also think you should hold off for as long as you
can.

## 8. Non-Production Environments Have Diminishing Returns

A more direct heading for this section would be **"Non-Production Environments
are Bullshit"**. Environments like staging or pre-prod are a fucking lie. When
you're starting out, they make a little sense, but as you grow, changes happen
more frequently and you experience drift. Also, by definition, your non-prod
environments aren't getting traffic, which makes them fundamentally different.
The amount of effort required to maintain non-prod environments grows very
quickly. You'll never prioritize work on non-prod like you will on prod,
because customers don't directly touch non-prod. Eventually, you'll be
scrambling to keep this popsicle sticks and duct tape environment up and
running so you can test changes in it, lying to yourself, pretending it bears
any resemblance to production.

## 9. Things Will Always Break

It's impossible, and in fact, undesirable, to avoid failure. **Lean into the fact
that failure is inevitable, and focus on how you respond to it**. This means
investing in a continuously improving incident response process. There's no
one-size-fits-all for every company and team, but you should have a good idea
of what to do when things go wrong, and you should have mechanisms in place to
learn from those situations and improve your processes. Invest in [Incident Analysis](https://www.learningfromincidents.io/). It's a huge field with lots of valuable tools and resources for
maximizing the return on investment when incidents occur (or don't!).

This is an area where Chaos Engineering can be really helpful. [Injecting failures into production](https://queue.acm.org/detail.cfm?id=2353017) can improve confidence in how to respond when a system
starts behaving in unexpected ways. [Game Days](https://www.gremlin.com/community/tutorials/how-to-run-a-gameday/) can be a particularly effective
way to allow a team of engineers to practice various outage scenarios.

## Conclusion

A lot of the beliefs outlined in this post are at least counter-intuitive, if
not somewhat controversial, but I'm nevertheless convinced that they're true.
That doesn't mean my mind cannot be changed, but it is unlikely. If you
strongly agree or disagree, [I'm on the internets](https://twitter.com/paulosman). I'd be very curious to hear
about your experiences.
