---
layout: post
title: "Communication skills > Technical skills"
date: 2017-02-19 11:38
comments: true
categories:
---

Speaking or writing, is something I thought was basic, something anyone with a basic education can do. I felt that I was done with honing my language skills after I've left school. If the school thinks it was good enough to graduate, it was good enough.

<!-- more -->

Early in my career, I was more interested in building my technical skills. Companies would ask if you knew X language, mastered the Y framework or wrote apps that did Z. Those skills were measurable. Either you wrote something with it or you didn't. It is much harder to measure communication skills.

Communication is the act of transferring information to another person. There are large variance in effectiveness. Imagine you have this fantastic idea of __Uber for Sailing boats__ in absolute clarity in your head. You want to pitch this idea to a group of tourists, hoping they would try sailing. Communication is like drawing a mental picture of what you see in another person's head. A skillful communicator would paint something like this in anothers mind:

![Rembrandt's Church in the store on the lake of Galilee](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/Rembrandt_Christ_in_the_Storm_on_the_Lake_of_Galilee.jpg/482px-Rembrandt_Christ_in_the_Storm_on_the_Lake_of_Galilee.jpg)

While an average one may only let the tourists think of this:

![](https://s3.amazonaws.com/static.liangzan.net/blog/6407942619_fe539a1580_m.jpg)

Your writing bridges the gap to your thoughts. The better you are, the more details they see, the easier they can understand you.

The technical world is awash with difficult concepts that needs better explanations. Take for example the `RAM`, how do you explain that to a five year old? I had difficulty understanding it when I was in my teens. My teachers calls it the `memory`. The word `memory` had connotations with storing information, which I naturally associated with our own memory. So what was the difference between the hard disks and the memory? Both store information correct? Why have two in a computer? This lingering question was never cleared up until after I wrote my first programs years later. The ability to pare down a foreign concept to something digestible is invaluable.

## Brevity in code and in documentation

There are bad technical writing everywhere. The biggest problem is they are often too brief. Preference for brevity in code has diffused into documentation. Let's take a look at how the official Scala docs attempts to explain [currying](http://docs.scala-lang.org/tutorials/tour/currying.html):

> Methods may define multiple parameter lists. When a method is called with a fewer number of parameter lists, then this will yield a function taking the missing parameter lists as its arguments.

The definition is correct. The example demonstrates its usage. But do you _understand_ when to use currying? Code should be brief as you are writing for the computer. But documentation are written for humans. Authors should strive to explain not only the how, but also why and when to use it. More horrors to come.

## Jargon-filled wishy washy

From [thoughtworks radar](https://www.thoughtworks.com/radar):

> The microservices style of architecture highlights rising abstractions in the developer world because of containerization and the emphasis on low coupling, offering a high level of operational isolation. Developers can think of a container as a self-contained process and the PaaS as the common deployment target, using the microservices architecture as the common style. Decoupling the architecture allows the same for teams, cutting down on coordination cost among silos. Its attractiveness to both developers and DevOps has made this the de facto standard for new development in organizations.

Do you know what the author is trying to say? Let me attempt to simplify it.

> Containers and Microservice complements each other naturally, yielding advantages. Low-coupling helps your infrastructure become more resilient. In the event of failure, the failure is isolated and does not bring down the entire infratructure. Being isolated allows teams to work independently, cutting down wasted time coordinating deployments. Its advantages has made it the standard for new projects in companies.

Start with the topic sentence and cut down on the jargon. Your readers will thank you for it. I learned that from _Style_. Similar to [good user interfaces](https://www.amazon.com/Dont-Make-Me-Think-Usability/dp/0321344758), clear writing don't make the user think.

## The mythical self-documenting code

Every developer I met likes to write APIs but hates to write documentation. They will spend herculean effort, meticulously designing an intuitive RESTFUL API that scales, but they will put in the least amount of effort necessary to write documentation for it. Even resorting to [auto-generating documentation](http://swagger.io/). They would claim their API is so well designed, the endpoints and parameters so well named, that it is self-documented.

Have you heard of Haskell?

I was looking for a XML library. I found one and wanted to learn how to use it.

``` haskell
fromContent :: Content -> Cursor Source

A cursor for the given content.
```

Sometimes I'm lucky - there is one sentence.

``` haskell
fromTag :: Tag -> [Content] -> Element Source
```

Most times I get a type signature.

Haskellers would often argue that the signature is good enough. Do you believe that, or do you think it is just excuses made to hide their disdain for writing documentation? Granted, this malady is widespread across all communities, but Haskell is in a plague that is.

Inevitably when emails start appearing asking what does that endpoint do, what does that parameter mean, you will realise that the time spent answering those emails could be better spent writing documentation in the first place. Perhaps it is because that employers look first for technical skills, and second on how well you communicate, that communication became a second citizen.

## Becoming the 10x multiplier

If you are good at communication, you naturally become a 10x multiplier of others. It is more valuable than being [the mythical 10x engineer](http://antirez.com/news/112). It is a skill that should be valued, which sadly I don't see. Things like onboarding new engineers to a new codebase, technical support overheads, all these go away with someone in your team who is a good teacher and patient enough to write. It is not hard to understand why as communication skill is impossible to quantify. Engineers when given a choice to work on their side projects or writing, would gravitate towards coding. It is what brings them to this profession. That is why we must insist that good engineers must not only build well, they must also communicate well.

How do you be better? Like every skill, you practice. Writing and speaking are but conduits to your thoughts. [Read](https://www.goodreads.com/) voraciously, read widely, and think for yourself if what they say is correct. For writing, I recommend [Style: The Basics of Clarity and Grace ](https://www.amazon.com/Style-Basics-Clarity-Grace-5th/dp/0321953304) by Joseph Williams. This book distils the process of writing well into easy to understand steps. Compared to [On Writing Well](https://www.amazon.com/Writing-Well-Classic-Guide-Nonfiction/dp/0060891548), _Style_ is better. The easy way is to dish out rule-of-thumbs, but he did not. There is a widely held misconception that passive voice is bad. In the right context, the sentence flows better with the passive voice. _Style_ shows you that. You should read both books nonetheless.

I usually write on Emacs, and there are tools which I use and would recommend. I use [ispell](https://www.gnu.org/software/ispell/ispell.html) to check my spelling. I use the [writegood mode](https://github.com/bnbeckwith/writegood-mode) to catch passive voices and weasel words. There are readability tests like [Flesch-Kincaid](https://en.wikipedia.org/wiki/Flesch%E2%80%93Kincaid_readability_tests) included in writegood mode which scores your writing. I also use [thesaurus.com](http://www.thesaurus.com/) to find the exact words for conveying my thoughts.

To hone your speaking skills, practice. Find a [Toastmasters](https://www.toastmasters.org/) chapter nearby, attend it and seek chances to speak. Toastmasters have impromptu sessions where you are given a topic to speak on the spot. Those are great exercises for thinking on your feet. In less than 10 sessions, you will lose the fear of public speaking. What is most valuable is you get feedback from others after every session.

There is this verse from [Tao Te Ching](https://en.wikipedia.org/wiki/Tao_Te_Ching):

> 知不知，尚矣；不知知，病也

Which means there are knowledge you know you don't know(like I know I don't know nuclear physics), and knowledge you don't know you don't know(like that missing breakthrough for robotics, if it exists). We should strive to know what we don't know, because it makes you aware of how little you know, and so you will find the reason to keep learning. I certainly hope I have planted enough seeds of doubt in your mind, enough to question if your communication skills is _good enough_.
