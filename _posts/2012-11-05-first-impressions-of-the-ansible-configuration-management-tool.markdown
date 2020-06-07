---
layout: post
title: "First impressions of the Ansible configuration management tool"
date: 2012-11-05 06:56
comments: true
categories:
---

While working at [Action.io](http://www.action.io), we decided to use [Ansible](http://ansible.cc) for managing our deployments. Previously we were using [Opscode Chef](http://www.opscode.com/chef/). We felt that Ansible suited our needs better. Let me illustrate why.

<!-- more -->

## What is Ansible?

Ansible is a [configuration management](http://en.wikipedia.org/wiki/Configuration_management) tool. Like Opscode Chef or [PuppetLabs Puppet](http://puppetlabs.com), Ansible helps you to manage the configuration on your servers. Without a configuration tool, system administrators would install, configure and update server software by hand. Configuration management tools help to automate that.

At the start, since all of us at Action.io were fluent in [Ruby](http://www.ruby-lang.org/) and Chef, our natural choice was Chef. We written [cookbooks](http://wiki.opscode.com/display/chef/Cookbooks) and [recipes](http://wiki.opscode.com/display/chef/Recipes), got hosted Chef running and were deploying with [Knife](http://wiki.opscode.com/display/chef/Knife). Happy days! But something didn't feel right.

## Chef did not suit us

Perhaps I am a Chef noob. I was never convinced on the pull workflow. After an update to the cookbooks on the Chef server, we had to wait for the Chef clients to poll for updates, to _pull_ changes from the Chef server and update itself. It is The Wait that feels wrong. Why do I have to wait? I want my servers to get updated _now_. Not 5 minutes later. What I did everytime is to ssh in to the servers and run ```sudo chef-client``` to make the server pull changes immediately. I asked some of my peers who also use Chef. Remarkably, they did the same too.

If me and my friends did that, it does not mean everyone does that. My point is the pull workflow does not feel intuitive. I'm not a veteran with 10 years of experience running hundreds of servers. Maybe there is a good reason to adopt a pull workflow. But for a small setup with less than 20 servers, it felt out of place.

Our next pain point is: Chef is resource hungry. Running a chef client process takes up precious memory and CPU cycles. For Action.io's case, those memory and CPU cycles could be better utilized for the users. Not waiting for an update that arrives Once In A While.

## Arrives Ansible

Here is my sales pitch for Ansible. Ansible pushes updates(like [Capistrano](https://github.com/capistrano/capistrano)). You need not wait for updates to happen. It cuts total deployment time. Especially in misson critical situations like An Outage. You'd prefer to be up in 5 minutes rather than 50 where 45 is spent waiting for the Chef client to poll(exaggeration of course). Sold?

Ansible installs _nothing_ on your servers. No idle client that does nothing but sit there and polls every hour. Think of the memory that Nginx could have used for serving requests. How about those CPU cycles that could be available for Postgresql to do indexing? Why keep that lazy bum around? Nginx and Postgresql will harbour resentment over time. Keep your star workers happy. Ansible keeps no such lazy bums. Sold?

Ansible is easy to grasp. Reading an Ansible playbook is like reading your Bash history. Your playbooks are completely written in [YAML](www.yaml.org). YAML comes with less markup noise. It is a wise choice. The choice of using YAML allows Ansible to be language agnostic. Ansible does not need fluency in a particular programming language. Which means the learning curve is low and it is easy to get proficient quickly. Furthermore, the Ansible paradigm is "I ssh in and run command Foo". That is exactly the same as hand building a server. Which means it should be dead easy to grasp. Another Big Plus is you know exactly what is going on. Ansible is running shell commands over ssh. The commands are not hidden under some abstract concept of knifes and puppets and flying saucers. Error messages will be clearer(hint: easy to debug -> less downtime -> more money). Being easy to learn means anybody could learn to deploy(hint: no need to hire a dedicated sys admin -> make your existing engineers work harder -> more money). Sold?

## Being young is not always good

A sales pitch had to make you believe elephants could fly, wash your laundry and perhaps deploy your server as well. Being that I am not actually doing Sales, I can be honest with Ansible's warts. And so the anti-sales pitch. Being a young project Ansible suffers from the common ailments of Poor Documentation and Non Existent Eco System. If I were using Chef, there are usually cookbooks available for common software. Not for Ansible. At Action.io, we hand wrote all of them. The mailing list is still the best chance where you can get your questions answered. Let's be fair, it's all part of growing pain. The Ansible eco system is not mature yet.

So what do you think of Ansible? Would you use it? Ansible as a tool is mature, but the eco system is not. It wasn't hard to hand write our playbooks. Nor did it take much time. A better eco system may bring ready made playbooks. We still have to customise it. To us, it is the other advantages that Ansible brought that made us decide to use it. I hope you are sold.
