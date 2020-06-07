---
layout: post
title: "How does FreedomVPN work"
date: 2016-05-29 05:54
comments: true
categories:
---

I just watched the Champion's League Final streamed live on my laptop. I watched it on [PPTV](http://www.pptv.com/) in Singapore. This is made possible by FreedomVPN(Which amazingly, does not have a landing page of its own where I can link to). Recently, I switched my ISP(Internet Service Provider) to [Viewqwest](http://www.viewqwest.com/). This far, so good. Their product FreedomVPN, has opened the door to enjoying Internet TV from most of the popular content sites of the world. To many, it is like voodoo. Let me try to explain in broad strokes how they did it. Disclaimer: these are my conjectures and has not been validated with Viewqwest.

<!-- more -->

## Just watch it

We didn't realise we had Freedom VPN at the start. How we discover it was amusing. I realised that we suddenly had more titles to choose from Netflix. We thought the Singapore catalogue has been beefed up. Only after discovering those new titles can only be seen when we are at home, we then realised it was Freedom VPN.

There is no need to configure anything. Just watch it. It works across your smart phones, media players and the web browser. It is indeed convenient. It quickly won us over. We now access titles available from the US catalogue of Netflix. We can view BBC, the Chinese media sites and so many more.

With the new content that is now available to my family, we ended our cable TV subscription. It is obvious that media content delivered over the Internet is going to be the norm. My children will never experience catering their own schedules around the TV's programme schedule, or the frustration of being restricted to watching it on the TV.

## When you press "Sherlock"

I had described what the user experience is like. Let me lay out the requirements.

- No configuration needed
- Connects to multiple content providers, each serving different countries
- Works on all devices

[Sherlock](http://www.imdb.com/title/tt1475582/) is a brilliant TV show that is not available in the Singapore Netflix catalogue but is, on the US one. When I select __Sherlock__, what really happens?

My web browser asks Netflix for __Sherlock__. The request has a problem. Where is Netflix? The expression __www.netflix.com__ does not say where it is. It needs an address, an IP address. To translate __www.netflix.com__ to an IP address, it asks a [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) server. When you're on FreedomVPN, you are asking Viewqwest's DNS server.

On my laptop which is served by Viewqwest,

```
$ nslookup www.netflix.com

Non-authoritative answer:
Name:   www.netflix.com
Address: some.ip.address
Name:   www.netflix.com
Address: some.ip.address
Name:   www.netflix.com
Address: some.ip.address
```

I can't divulge what the IP addresses are, but I can tell you where they point at: somewhere from Viewqwest's office, in Singapore.

![Where Netflix is from FreedomVPN](https://s3.amazonaws.com/static.liangzan.net/blog/singapore_netflix_lookup.png)

On a non-Viewqwest network,

```
Non-authoritative answer:
www.netflix.com canonical name = www.geo.netflix.com.
www.geo.netflix.com     canonical name = www.us-west-2.prodaa.netflix.com.
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.34.178
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.37.191
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.40.137
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.4.148
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.15.237
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.22.153
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.23.124
Name:   www.us-west-2.prodaa.netflix.com
Address: 54.214.28.210
```

It is pointing unmistakably to Netflix servers. What does this mean?

> Masquerading: figurative sense of "false outward show" is from 1670s.

Requests are inherently gullible. They are led to believe that Netflix is really at Viewwqwest's offices. Off they went. Does anyone really believe that the files of __Sherlock__ are kept at Viewqwest's office? It isn't. Now, our request needs to be satisfied with __Sherlock__. Where do we get __Sherlock__? From Netflix obviously.

Why would Netflix give it to the request? Remember, the request came from Singapore. __Sherlock__ is _not_ available for Singapore viewers. That is why we got to tell Netflix that the request did not come from Singapore, but from the US.

![Netflix activity](https://s3.amazonaws.com/static.liangzan.net/blog/netflix-access-activity.png)

Look at my screenshot. This came from my activity logs in Netflix. I watched Netflix from US without me physically in the US. Where do the IP addresses point to?

![Viewqwest US IPs](https://s3.amazonaws.com/static.liangzan.net/blog/viewqwest-us-ip.png)

Coming back to our request, what happens is our original request is routed to Viewqwest offices in the US. It then _pretends_ that the source is from the US, sends to Netflix, gets __Sherlock__, routes back to the Singapore office, and then finally back to the end user, me, happily watching Sherlock duel with Moriarty. FreedomVPN listens actively for requests to their list of supported content websites. It then performs this form of masquerading to geo-unblock content for you. This is FreedomVPN.

## Cat and Mouse

With Netflix clamping down on proxies, Viewqwest through Freedom VPN is the only vendor that I know of, that is able to circumvent content geo-blocking. They are treading a thin line.

Does it infringe terms of use? I don't know. Content providers won't be happy knowing that their content can be accessed in areas where it is not supposed to. Knowing is one thing, __enforcing__ is another. Is it worth Netflix's time to spend time and resources to go after FreedomVPN? Maybe not, since Viewqwest subscribers are not many enough to warrant spending the energy. I'm sure they have better things to do. As long as FreedomVPN continues to be a niche player, it should continue to stay under the radar. Perhaps that is why it never got its own landing page.

I see some Viewqwest customers complain that FreedomVPN is not stable. It will never be. Accept that this is a cat and mouse game. This solution is a workaround, and will never work cleanly unless geo-blocking is abolished. But for the convenience of watching __Sherlock__ with one click, I'm wiling to accept that good enough is _good enough_.
