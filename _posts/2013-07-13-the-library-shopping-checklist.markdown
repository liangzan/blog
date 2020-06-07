---
layout: post
title: "The library shopping checklist"
date: 2013-07-13 05:59
comments: true
categories:
---

![Shopping. By Epsos.de](http://farm6.staticflickr.com/5062/5652699228_68587eb26c.jpg)

Library shopping is like second nature to developers. A library refers to packaged code like [Ruby gems](http://rubygems.org), [NPM packages](http://npmjs.org) or [Python packages](https://pypi.python.org/pypi). Along the years, I do these evaluations unconsciously. I thought it'd be good to put this mental checklist down in writing.

<!-- more -->

## Does the library fulfil your requirements?

My first filter is to look for libraries that fulfil what I need to do. Different libraries have different features. The clearer you are on what you need to do, the easier it will be to filter which library is more suitable.

## Is the library maintained?

Is the last commit recent? If the last commit is less than 6 months old, I would view it as recent.

Does the documentation or website say that the library is not maintained anymore? Sometimes authors do put up notices that a project is abandoned. Sometimes if the official library is not maintained, there may be forks that may have patches. You could use those instead.

Are the issues fixed promptly? Take a look at how fast issues gets fixed. It is an indicator of how well maintained the library is.

## Will the library be maintained?

Are there more than one contributor? If there are more than one, it is a good sign. A solo contributor is less preferable, as no peer pressure makes it easy to abandon a project.

## Is the library compatible with your platform?

Are you on the JVM, on Windows or on Ruby 1.8.7? The library may not support your platform, so you must check.

## Is the library well documented?

The documentation has to address these concerns before I would see it as well documented

 - Does it have an API reference?
 - Does it address how to install?
 - Does it address how to uninstall?
 - Does it address how to use it?
 - Does it address how to update older versions?
 - Does it explain how it works?
 - Does it explain all the features?
 - Does it have examples?
 - Does it have a change log?

You have to take into account the quality of the writing. Clarity and succintness is what I look for.

You also have to take note if the documentation is out of touch with the changes in the code. Check for the commit dates of the documentation.

## Is the library well written?

Read the code. Is it clean? Is this the quality of code you would like to have in your project?

## Is the library well versioned?

Check the history of versioning. Does the library adhere to [Semantic Versioning](http://semver.org)?

Some libraries don't. If they don't, it makes updates harder.

## Does the API help more than hinder?

Take a closer look at the usage of the library.

Do you need to do a lot of configuration work to get it working? Or does it work out of the box?

Does the API force you to change your existing code? Or does it fits in beautifully?

Is the API intuitive?

And there is a matter of taste. Does the API feel enjoyable to write in?

## Does the library have tests?

Never choose a library that have no tests. You will end up as a guinea pig.

## Is the library's license permissive enough?

There are a variety of [software licenses](http://en.wikipedia.org/wiki/Software_license). Note which license the library is using. Can your project use a library that uses that particular license?

## Is the library mature?

How long has the library existed? Is it years, months or weeks? Prefer the long-lived ones to the young ones.

How many users are using the library?
How many downloads, watchers does the library have?
Are there blog posts covering the library?
Does it have many contributors?
Does it have many people reporting issues?

Of course, the more mature the library is, the better.

## Does the library have dependencies?

Certain libraries have heavy dependencies. Take for example, [Chef server](http://docs.opscode.com/chef_overview_server.html) has [Couchdb](http://couchdb.apache.org), [RabbitMQ](http://www.rabbitmq.com) and [Merb](http://www.merbivore.com) as its dependencies. Generally, prefer libraries who install minimal extra dependencies.

## Does the library break existing code?

Take a close look at how the library works. In the case of Ruby, a library may monkey patch common classes. It may break your code. So do be careful. If you have a comprehensive test suite, it is time to put it in use.

## Is the library stable?

If the library has reached stability, and the API don't change much, that is good. It means you don't have much of a maintenance cost. There is no need to keep updating your code to keep up with the library.

## Does the library have alternatives?

This is the cost of switching. Over the long term, there is always a chance that a library may get abandoned. What are the other alternatives? It is always good to have _competition_ serving the same function.

## Do you understand how it works?

You should always strive to understand how it works below the hood. This will help immensely when problems occur, or when you need to further customize it. Prefer libraries that does one thing well, than those who do many things.

## Is the performance good?

If the function that the library is serving may be intensive, it would be good to pay attention to its performance. Libraries such as [Resque](https://github.com/resque/resque) and [Sidekiq](http://sidekiq.org/) serves similar functions. The only perceptible difference is their performance.

## Is it simpler to use it or write it yourself?

If the functionality is simple enough, it might be a better choice to implement it yourself. Implementing yourself has the advantage that you understand how it works. And you'll be free from hassle of keeping the library up to date.

I hope this checklist helps to you to do your library shopping.
