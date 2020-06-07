---
layout: post
title: "How SmugFTP came into being"
date: 2011-07-23 16:35
comments: true
categories:
---

[SmugFTP](http://smugftp.com) is my side project. It has been out in the wild since this January. Why did I build it? The reason is I find uploading painful. I wanted to scratch my own itch.

<!-- more -->

I signed up for [Smugmug](http://www.smugmug.com) after my wedding. My wedding photos were precious. My wife and I wanted to share it with our friends and family. We wanted to back our photos up. We chose Smugmug. Everything was good. Except the uploading. An upload usually take many hours. It's common to leave you computer running overnight to complete the upload. The thing is, uploads fail. When it fails, it stops. So you have to check it periodically. If it fails, restart it. I actually felt a sense of relief and accomplishement once the uploads are complete. I wasn't happy with the web uploaders. The default uploader doesn't work. I chose the Java one. It felt clunky. The engineer in me knew that there'e got to be a better way to do this. So I went to the Smugmug wiki to find uploaders that others have written. Unfortunately there's none that look good. Feeling disappointed, I went to the Smugmug user voice page. And there it was, a suggestion for FTP uploading. I wanted FTP. I voted. That was when I thought to myself, FTP is not too hard to build, why can't I build it?

And so I started building it. Smugmug had an API. After doing a little research, I knew FTP to Smugmug is possible. And not too hard too. The first beta release was out in a month. Immediately, there's interest from other users. Slowly, word spreaded and the project grew. It was until after 6 months or so that I felt that SmugFTP is finally stable.

There're several things I learnt

1. Testing is very important. Try to make the tests as similar as the actual environment
2. Don't be ashamed to release a buggy product. SmugFTP was buggy for a good 6 months. It caused many users inconveniences. But they still forgave me.
3. Be nice to your users. If you're nice, people reciprocate. I always apologised for failed uploads in the early days. And I went out of the way to fix the mess that the bugs created. All of them appreaciate it. And I had a fantastic relationship with some of my users as a result of that.
4. Solve a real problem. If you're solving a painful problem, users will find you. They will stick with you, forgive you and spread word for you. I find too many people are building stuff that do not solve actual problems.
5. Development always take longer than you think. The production environment always have surprises for you.
