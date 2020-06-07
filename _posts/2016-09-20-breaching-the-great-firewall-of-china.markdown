---
layout: post
title: "Breaching the great firewall of china"
date: 2016-09-20 06:15
comments: true
categories:
---

It has been some time since I stepped into China. The great firewall has since advanced. This is a short guide to how to breach the [great firewall](https://en.wikipedia.org/wiki/Great_Firewall) meant for the technically inclined. This guide is accurate as of 2016.

<!-- more -->

## Use OpenConnect

In short, [OpenConnect](http://www.infradead.org/ocserv/index.html) works. I've tried OpenVPN and ShadowSocks. Only OpenConnect works.

I used an open source project, [Streisand](https://github.com/jlund/streisand) to set it up. Streisand needs a server to set up from. Any of the major cloud providers or any webhost providing Ubuntu 16.04 server will do. I used [Vultr](https://www.vultr.com/).

Streisand runs on [Ansible](https://www.ansible.com/), which I'm familiar with. First, you need to `git clone` the project to your local machine. I updated the IP address under the __inventory__ file of the Streisand project to my Vultr server. Next, run the script. It took about 20 minutes for it to complete.

Once completed, a folder of HTML documents is generated on the local machine. The documents outlines the steps to connect to the remote server using the various protocols. The instructions were clear, and even included links to the binaries. Each document is unique to the server. The certs, passwords are all unique to the server. Most importantly, it worked. I was able to access Google, and the various blocked sites from China.

I hope this helps. For the non-technically inclined, unfortunately I wasn't able to find a company that provides OpenConnect commercially. Perhaps this would get better in future. I'm sure there is a market for such a service.
