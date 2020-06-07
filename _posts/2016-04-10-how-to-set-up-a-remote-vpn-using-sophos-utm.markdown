---
layout: post
title: "How to set up a remote VPN using Sophos UTM"
date: 2016-04-10 06:49
comments: true
categories:
---

My office has a Sophos UTM. It acts as a firewall for the office network. We also use it as a VPN server. It allows us to access the machines in the office network. More importantly, it allows remote users to access the servers. Our servers have access restricted to a white listed list of IP addresses. This way, we only open up the white list to the office network. This allows our people to work from anywhere. We don't have to keep updating the white list of IP addresses. When I tried setting it up, I could not find articles documenting how to do it. Hopefully it will point you in the right direction.

<!-- more -->

## Setting up the Remote VPN

To set it up, I referred to the manual from Sophos. Their [knowledge base](https://www.sophos.com/en-us/support/knowledgebase/119209.aspx) contains the administrative guide. Download the administrative guide for your version of UTM.

The relevant chapter is __Remote Access > SSL__. There also plenty of articles and videos on the web on how to set that up. I won't repeat them so as not to bore the reader.

## Masquerading traffic from the office

My aim is when I log into the VPN from a remote location, my traffic should masquerade as coming from the office. To the outside world, my requests comes from the office network's IP address. To do that, I need to add a Masquerade rule. Go to __Network Protection > NAT__, create a new Masquerading rule by clicking on the button __Create Masquerading Rule__.

![Sophos UTM Create New Masquerading Rule](https://s3.amazonaws.com/static.liangzan.net/blog/sophos-utm-create-new-masquerading-rule.png)

Select the network definition which represents the remote SSL users(you would have defined them when setting up the remove VPN). Select __WAN__ as your interface, as you would like the traffic to go out. Click __Save__. Enable the rule by turning on the rule in the toggle button. Your rule should look like the screenshot below. You're done. Hope this helps.

![Sophos UTM Masquerading Rule]( https://s3.amazonaws.com/static.liangzan.net/blog/sophos-utm-masquerading-rule.png)
