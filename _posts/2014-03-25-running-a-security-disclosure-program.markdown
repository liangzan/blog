---
layout: post
title: "Running a security disclosure program"
date: 2014-03-25 05:59
comments: true
categories:
---

![Security. By perspec_photo88](http://farm8.staticflickr.com/7308/11406985424_457c44045f_m.jpg)

A few months ago, I started a security disclosure program for my [employer](https://www.dropmyemail.com). It is definitely beneficial for us. I'd like to share some of our experiences running a security disclosure program.

<!-- more -->

## What is a security disclosure program

A security disclosure program is a program is an open invitation for security researchers to find vulnerabilities in your application. If they find something, they are encouraged to disclose to us and allow us time to fix it before going public. Your company may choose to offer a reward. It could be monetary or an acknowledgment on the Hall of Fame. Many companies run such programs. Good examples include Google, Facebook and Github.

To the companies running such a program, it gains a group of enthusiastic volunteers who will scrutinize their apps for vulnerabilities. It is far more cost effective than having in-house employees do the same. For the security researchers, it is a form of marketing or income for them. It is a win-win scenario for both parties.

## The influx

We first posted the security page detailing our disclosure program. Within days, security researchers found our program without us telling anybody about it. That is our first surprise. Very quickly, more and more security researchers came without prompting. There was a visible spike in our traffic. [BugCrowd](https://bugcrowd.com) knew about our program and added us to their [list](https://bugcrowd.com/list-of-bug-bounty-programs) of bounty programs. The security researchers come from all over the world. The quality of the reports varies widely. Through their reports we managed to uncover no less than 5-6 serious vulnerabilities. That alone was worth it.

After the initial rush, things will begin to quiet down. You will feel more confident about the security of your app.

## The cons

There are _a lot_ of reports to respond to. It is very similar to handling support. It creates an additional load on the team. Every report has to be investigated and judged if it is a real vulnerability or not. Most of the time the reports cover fairly trivial vulnerabilities. Some may not agree with you on whether it is a real vulnerability or not. You definitely need someone in your team to be able to judge the vulnerabilities.

During this period expect various parts of the app to break. We have researchers running automated tools against our apps. There was a huge spike in errors. No doubt it caused some inconveniences.

I think rewards are a good motivator. Think carefully what reward you want to give. Not all rewards are the same. A Hall of Fame only requires a page update. Whereas a T-shirt costs you money to print, time to package and money for postage. Does your team have the time to send out dozens of shirts every week? Do you have enough shirts to give out? If it is a monetary reward, expect arguments with you on your judgment.

## In retrospect

Security disclosure programs are definitely worth the cost of running it. But don't take for granted that having such a program in place means your app is secure. Most of the security researchers only do a shallow dive. They don't spend too much time since the payoff is not there. You should still get a professional security consultant to perform an audit.
