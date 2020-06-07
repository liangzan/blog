---
layout: post
title: "First impressions of Leap Motion"
date: 2013-02-06 17:10
comments: true
categories:
---

For those who don't know what [Leap Motion](http://www.leapmotion.com) is, take a look at this youtube video.

<iframe width="560" height="315" src="http://www.youtube.com/embed/_d6KuiuteIA" frameborder="0" allowfullscreen></iframe>

I got the Leap Motion hardware and SDK by participating in their [developer program](https://leapmotion.com/developers). I was the lucky few that was selected. They informed me through email. A few weeks later, the Leap motion device is mailed to me, free of charge. It came in a dull black cardbox box. Within the box lies the device, a cable, and a card bearing a message from the founders.

<!-- more -->

## First impressions of Leap

![Leap Motion box](https://s3.amazonaws.com/static.liangzan.net/blog/leap-motion-box.jpg)

It is very small and light. One side etched the Leap logo. The other lies the sensors. When you power the device, three red bulbs reveal themselves. That is where the magic happens. Powering the device will does nothing, you need to install the software. There is a SDK provided for Mac and Windows platforms. It comes with a set of examples and documentation to help you get started. Once you connect the device up, run the Leap app and you'll be ready to go.

![Leap Motion unboxed](https://s3.amazonaws.com/static.liangzan.net/blog/leap-motion-unboxed.jpg)

The Leap SDK gives you several debugging tools. There is a visualizer which displays a trace of your fingers as you move.

<iframe src="http://player.vimeo.com/video/57635349" width="500" height="281" frameborder="0" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe> <p><a href="http://vimeo.com/57635349">Leap Motion Visualizer</a> from <a href="http://vimeo.com/user14137242">Graham Gaylor</a> on <a href="http://vimeo.com">Vimeo</a>.</p>

It is backed by a grid, so you can literally trace little grids with your finger. Accuracy is touted as Leap Motion's strength. That is no lie. I can easily trace the small grid with my clumsy finger. It is nothing short of amazing. Besides your fingers, Leap traces anything that resembles a stick. I don't sense a lag. Even if I move quickly, Leap is able to capture it. I could tell Leap is computationally intensive. My computer fans are always in ovedrive when I use Leap.

You could play with the sample apps. They are mostly simple _hello world_ style apps that shows you how to call the APIs. Sadly, there are no fruit ninja apps for you to play with. The Leap Motion team built an input emulator to interface with games like fruit ninja. They promised to open source it though. The Leap SDK provides bindings for Java, C++, C#, Obj-C, Python and Javascript. I believe more will come. The API itself is fairly primitive. It gives you access to the raw data. You will get data on the position of each pointer, the orientation and the movement. There are no gestures or other abstract APIs.

## Development experience

Like a kid with a new toy, I was eager to build applications with it. I chose to try the Javascript API first. The Leap SDK runs a web socket server which allows the Javascript bindings to access the data. Usage of the API is boringly simple. It is an infinite loop which gives you a frame every iteration. The frame is a JSON object which contains the data on the pointers.

The [first leap experiment](https://github.com/liangzan/leap-demo/tree/master/particles) I did was to replicate Mike Bostock's D3.js [particles](http://bl.ocks.org/1062544). I got a trail of particles to follow each pointer.

Gestures naturally come to my mind. The SDK has no provision for gestues yet. I have to recognize it. And that is an _Articifial Intelligence_ problem. I looked for gestures related libraries to build on. And I found the [$1 unistroke recognizer](http://depts.washington.edu/aimgroup/proj/dollar/). They had a Javascript implementation. I used it to recognize gestures in my [next leap experiment](https://github.com/liangzan/leap-demo/tree/master/gestures). It worked. But it has caveats.

A gesture has a start and an end. Leap runs in an infinite loop. I had to define a start and end. In my gesture experiment, I defined 2 states: fist and point. The fist state literally means clenching into a fist. It acts as a blank state which I use to define the end points of the gesture movement. The point state means an extended finger. In that state, movement is captured. While my experiment is able to recognize gestures, it did not do so cleanly. When I change between a clenched fist to a pointing finger, there are some jitters unavoidably. That polluted my gesture shape. It is very hard to form a clean shape. There should be a better way to define the end point of a gesture.

## Conclusion

As I presented my demo to the local javascript meetup group, everybody was visibly excited by Leap Motion. Leap Motion is an amazing product. Invariably, we wondered what potential applications could be built on Leap Motion. Games are an obvious application. Minority report styled navigation is another. For a long time we have been stuck with the keyboard and mouse. Then touch devices exploded onto the scene. After playing with Leap Motion, I firmly believe it is the dawn of another era of gestures styled devices. I'm living in exciting times.

## Update

Follow the discussion on [Hacker News](http://news.ycombinator.com/item?id=5179335)
