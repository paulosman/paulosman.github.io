---
layout: post
title: Questions I Keep Asking
---

Software is a big space and in the span of a career, you can move in a bunch of
different directions, all interesting to different people. As I approach my
20th year as a professional software person, I thought it’d be interesting to
reflect on where my career has taken me so far, and how I hope to spend the
next 20 years. I found it useful to write down questions I keep
coming back to and use them as a guide for my continued learning. I’ll be
interested to come back to this post and see how it compares to whatever
"future me" gets up to.

Some of these questions have answers and I just need to learn more about them.
Others are active research areas with no “right” answer. Regardless, they’re
things I keep thinking about, reading about, and occasionally applying as a
practitioner.

Of course, these aren’t the only things I’m interested in, they’re what I think
about the most. If anything, this exercise was a good way to make clear the
areas I’m not interested in enough to dedicate serious time to. For example, I
think that data visualization is an amazing topic. In a world with infinite
time, I’d love to dive into it, but probably not in this career. So, with that
said, here are my questions:

## How can we ensure that data flows through a system in ways that are performant, scalable, and reliable? How do we verify the integrity of data, especially in increasingly large-scale and complex data systems?

This is a broad area, but it’s useful as a guide. My core technical interests
gravitate towards backend engineering and this is why. I think that most of
what we do when working on 24/7 internet services boils down to accepting
requests to retrieve or mutate data. Retrieving data necessitates that we do it
quickly and correctly for some definition of correct (I’m really interested in
when that definition becomes flexible, see Question 4 below). When mutating
data, we want to ensure that a write is either accepted or rejected, i.e., the
operation is reliable. It’s rarely acceptable to mysteriously drop writes, it's
sometimes acceptable to repeat writes, and often it’s required that writes
happen in a specific order. This is all trivial with one process working at a
time but gets really hard when you have large numbers of concurrent operations
operating on more data than can possibly fit on one computer, and in a world
where you have to be able to tolerate the failure of one or more computers.

The fun thing about this question is how broad it is. You can start at a really
high level, with systems design concepts, and deep dive in various directions.
For example, load distribution and queuing theory are complete topics in their
own right. You can also study the data structures used to store data in memory
and on disk, to the protocols used to make decisions about how and where to
store data, all the way down to lower-level concepts like filesystem and
network performance. At every path, there are tradeoffs to consider and I doubt
anyone could ever be considered an expert in all these areas, but it’s really
fun stuff to think about and has real implications.

## How can systems be built to maximize operability? What techniques can be followed to allow operators to observe the behavior of a system, make decisions about its operation, and act on those decisions?

I want to help make life better for those of us who keep things running, as
well as the people who depend on the services we build and operate. What are
the properties of a system that make it easier to operate compared to the
properties of a system that operators fear? Luckily, I [have a job](https://www.honeycomb.io/) that allows
me to work directly on one aspect of this problem: Observability. Observability
refers to the degree to which a system’s internal state can be inferred by
looking at external outputs. Doing this well requires a lot of thought and
context.

This question also touches on complexity in systems. It’s intuitive, but often
wrong, to try and remove complexity — as system designers, we have this impulse
to try and hide complexity, but this can often create dangerous situations for
operators. Black boxes can have interesting, negative effects. This video,
about [why cars rarely crash into buildings in the Netherlands](https://www.youtube.com/watch?v=Ra_0DgnJ1uQ), explains it
rather well, I think. In software systems, operators often need to be able to
respond to operational failures never considered by the designer. Systems can
fail in fundamentally surprising ways, and operators need to be able to act on
whatever they are seeing.

I also think about the tunability of a system. Is it possible to affect the
internal state of software without deploying a change? For example, feature
flags are a useful way of toggling functionality and can be used effectively to
handle unexpected failures or conditions that degrade functionality. Traffic
control is another area where good complexity can be introduced. The ability to
direct small amounts of traffic to alternate versions of running software can
be tremendously helpful to operators responsible for rolling out changes
safely. Being able to make quick decisions about configuration changes, for
example, the version of a piece of software that a part of a system should run
is crucial in my experience. Chaos engineering is another area where I think
introducing complexity can be helpful.

## How do operators learn about the systems they are responsible for? How does that knowledge help or hinder them in their duties? How do they teach others who are less familiar with the system? How do teams coordinate and make decisions during high-stress incidents?

I believe these are classic [Cognitive Systems Engineering
questions](https://www.sciencedirect.com/science/article/pii/B9780444705365500063).
Our software systems don’t run themselves, [no matter what certain vendors
might want you to
believe](https://en.wikipedia.org/wiki/Artificial_Intelligence_for_IT_Operations).
People keep systems running, and people are constantly doing things, constantly
responding to things, and constantly changing things. Yes, change fail rates
are a real thing, but mostly, these the actions that people take are what
prevent outages or prevent outages from being worse than they are.

Have you ever worked with someone or a group of people who seemed to have an
intuitive sense of what needed to happen when a certain alert fires or a graph
doesn’t look right, or when a customer complains about a bad experience? Have
you noticed how much you can learn by just being around that person or on their
team? I’m fascinated by how operators obtain, continue to shape, and share that
knowledge. I’ve heard this process referred to as sense-making and I think it’s
an under-appreciated aspect of how we do our jobs.

I also know that we generally have different mental models of systems, and it’s
usually true that none of them are correct, so the ways in which we use those
different mental models to build shared understanding is fascinating to me.

People have [written
extensively](http://lup.lub.lu.se/luur/download?func=downloadFile&recordOId=8084520&fileOId=8084521)
on this subject, specifically in the context of
software operations and outages and I want to dig in a lot more.

## What are the tradeoffs when considering the needs of consistency, availability, and scalability? In order to guarantee coordination, is it necessary to introduce limits that interfere with a system's ability to scale? When is coordination unnecessary?

These are classic distributed systems questions and I find them really fun to
think about. They come up in practical applications from time to time. If you
think about data storage or any part of a distributed system that has to make
decisions about how to handle a certain kind of operation, we run into some
interesting constraints when considering needs of scale and availability. By
scale, I mean handling volumes of data that can’t possibly be stored on a
single disk, and by availability, I mean that the system will need to tolerate
individual node failures, so data will have to be replicated. In such a system,
how many nodes have to agree that a write has been accepted before a client can
consider the operation successful? When adding nodes to a system, how is the
performance impacted? Is it possible to create a system that requires no
coordination, but can make safe guarantees about consistency? This is an active
research area, and it’s fun to read papers from companies like Google,
Facebook, and Amazon to see how practitioners are thinking about the various
tradeoffs or fun ways to cheat. There seems to be a lot of back and forth
between academia and industry here. [CRDTs](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type), for instance, is an attempt to get
around the need for coordination while guaranteeing strong, eventual
consistency. The Google Spanner team is no doubt doing some really interesting
work here. It’s fascinating and I feel like you could spend a lifetime learning
about all the work being done in this area and still not be caught up.

## How do organizations learn? How do organizations internalize information necessary to guarantee their future success?

So you have a large scale data-driven system that incorporates a number of
design decisions intended to prevent or isolate failures. You have groups of
people who are constantly learning about and changing the system and
coordinating their efforts in surprising and remarkable ways. How does an
organization internalize this knowledge and avoid or make less likely certain
organization failure modes? How do new members of a team learn about all the
folk knowledge held within an organization? I’ve worked in organizations that
were dangerously dependent on individuals, and not able to withstand
commonplace things like vacation or attrition. These kinds of conditions are
hell and lead to burnout for individuals, deteriorating quality for a product,
and less reliability for users. I’m fascinated by the differences between teams
who disseminate this knowledge well and teams that do not.

## The next 20 years

Reading all of this, I could probably summarize my interests as: “building
large scale distributed data systems that are safe to operate and discovering
ways that teams internalize and share their operational knowledge.”. It’s fun
to break it down further though, and useful as a guide for learning. As I
described earlier, software engineering is a huge space with a seemingly
infinite number of paths to explore. It was a useful exercise for me to write
down the questions that seem to guide my learning and my career. I hope the
next 20 years of my career afford me more opportunities to study the areas I’ve
outlined, learn from my peers, mentors, and coworkers, and generally improve
the software I work on. I feel incredibly lucky to work in a space with so many
intellectually challenging problems, while also getting to help build products
that people find useful.
