---
layout: post
title: Post mortem of Notifymode
date: 2012-10-15 06:31
comments: true
categories:
---

In early 2012, I started Notifymode. Notifymode does application monitoring for [Node.js](http://nodejs.org) apps. Notifymode could profile the functions and track CPU and memory usage. It provides a high level overview of how the Node.js app is doing. It was a bootstrapped project. I didn't take any money. So why did I choose to build Notifymode?

<!-- more -->

## Motivation behind Notifymode

I had a Node.js app: [SmugFTP](http://smugftp.com). I had performance problems. There were few tools to help me diagnose where the problem was. The best I could find was the V8 profiler. V8 profiler gave me a [profiled report](https://gist.github.com/833370) which made no sense to me. I wanted a tool like [New Relic](http://newrelic.com) for my Node.js apps. New Relic did not support Node.js. Why not build it for fun and profit?

## I was _sure_ it would be useful

How could analytics not be useful? How could Node.js not be gaining traction? In fact Node.js is gaining traction at a frightening pace. [Google Trends](http://google.com/trends) shows the number of searches for Node.js outpacing [Rails](http://rubyonrails.org).

<script type="text/javascript" src="http://www.google.com.sg/trends/embed.js?hl=en-US&q=node.js,+ruby+on+rails&cmpt=q&content=1&cid=TIMESERIES_GRAPH_0&export=5&w=500&h=330"></script>

I'm sure there would be more Node.js apps released to production in time to come. Analytics for an up-and-coming platform? How could this idea be wrong?

I was confident that I am right. I don't need to do any of that [customer development mumbo jumbo](http://steveblank.com/category/customer-development/). I should just build it. For the next 3 months, I poured time and energy into building Notifymode.

You just need to add one line to your existing Node.js module to hook up to Notifymode's agent.

``` javascript
var io = require('socket.io').listen(app);
var profiler = require('notifymode-client').Profiler;

// one line here to hook up
var profiledSocketIO = profiler.profile(io);
```

The agent pushes data to the server. Notifymode then shows the time each function took, memory and cpu load.

## Build it and they will come

I submitted to [Hacker News](http://news.ycombinator.com) and [Reddit](http://reddit.com), though it did not make either's front page. I tweeted about it. My friends retweeted about it. I gave talks about it at the local Javascript meetup not once, but a few times. I wrote blog posts about using it with [Socket.io](http://socket.io) and [Express](http://expressjs.com).

The first few months was bad. I thought Notifymode was useful. I'm sure once people know about it, they will sign up. That was my first conjecture. So I started a [technical blog](http://node.jsreadme.com) on Node.js. Ideally that will funnel users in. In the process I realized putting out quality articles regularly demands significant effort. Blogging is very hard. I could not capture enough organic traffic. It was discouraging.

My next conjecture was: Notifymode is too general. Users probably wanted a specific agent which can push out app specific information. For example, Express users want to know the request time. Running times for individual functions is too granular to be useful. So I wrote customized agents for Express. I released it, blogged about it, and talked about it at the local Javascript meetup. Submitted to Hacker News and Reddit, and did not make either's front page again. And I waited for the flood of users to wash me away.

I kept Notifymode running for 6 months. At the last count, Notifymode had 20++ users. Some of them were spam bots(I did not put up a captcha). Some of them were friends. I couldn't bear to count how many real users there were. It was discouraging.

## Experience is merely the name men gave to their mistakes

> Tell me again was it love at first sight.
> When I walked by and you caught my eye.
> Didn't you know love could shine this bright?

![By Millzero Photography](http://farm4.staticflickr.com/3254/2408535634_f9953a5dbf_m.jpg)

Do you believe in love at first sight? We fall so deep in love with our ideas that we have this unwavering faith that it will work out. Despite numerous warnings from [Dear Aunt Agony](http://www.startuplessonslearned.com) that you should go out on a [few dates](http://theleanstartup.com) first, you insist that this is The Idea that you are going to marry. Well if you don't listen to Auntie Agony, you are most likely to end up in agony. Like me.

I was lazy to arrange first dates with my Idea. We got married too hastily. Now I'm divorced. At least I don't have to pay alimony. Don't be like me. Here are some [links](http://blog.asmartbear.com/) for [marriage](http://www.bothsidesofthetable.com/) [advice](http://www.avc.com/a_vc/).

Another thing I realized is: marketing is harder than coding. We geeks tend to view a product as 90% coding. The other 10% is the mumbo jumbo [marketing](http://en.wikipedia.org/wiki/Search_engine_optimization) [stuff](http://en.wikipedia.org/wiki/Search_engine_marketing). In reality, coding is the 10% while the other mumbo jumbo stuff takes 90%. Why? Coding yet another [Pinterest](http://pinterest.com) clone is easy. You know if you write this code in that logic, it will work. Getting 100 users? Where do you start finding them? It is hard because it is not definite. We geeks tend to dismiss anything non-intellectually stimulating as easy. Marketing, though technically easy, in reality is a lot of grunt work. No matter how useful your product is, nobody is going to use it if they have not heard of it. Notifymode taught me that I should have put in [more effort on marketing](http://www.kalzumeus.com/2009/12/31/engineering-your-way-to-marketing-success/) early on.

Granted, I've read this advice so many times from other [experienced](http://blog.asmartbear.com/quotes-startup-founders.html) [people](http://brooklynhacker.com/post/29901112213/what-a-hacker-learns-after-a-year-in-marketing). Why do people still make the same mistakes? It's like asking [Marco Polo](en.wikipedia.org/wiki/Marco_Polo), "How does Beijing looked like?". Marco Polo could tell you stories about festivals or paint you nice pictures of the palace. It is an approximation. You have to experience the real thing to get it.

## You're supposed to fail sometimes

At least I tried. I picked up a host [of](http://www.opscode.com/chef/) [technologies](http://backbonejs.org/) along the way. I made mistakes, and I learned. The only thing which I thought I did right was not to seek funding. I'm not sure if I closed Notifymode too early. Could things have turned out better if I persisted? You never know.
