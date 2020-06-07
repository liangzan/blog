---
layout: post
title: "Writing an ebook"
date: 2016-09-09 06:13
comments: true
categories:
---

I spent the past 3 months writing an ebook: [EverydayXPath](http://www.everydayxpath.com). The idea came as I needed to write a [Selenium](http://www.seleniumhq.org/) script. Naturally I needed to use [XPath](https://en.wikipedia.org/wiki/XPath) for selecting nodes on the web page. As I was googling for solutions, documentation on XPath felt inadequate. I needed good examples and coherent explanations for the various operators. XPath and CSS are the two common query languages for querying XML/HTML documents. I felt there is a opportunity for a niche product. Let me share the setup I used for writing the ebook.

<!-- more -->

## Writing the content

Writing an book is a lot of work. I spent an hour a day, about 3 months in all to complete it. The first part is research. Though I knew enough to write valid XPath expressions, I did not know it deep enough. I set out to read more about XPath. I read the [W3C specifications](https://www.w3.org/TR/xpath/). There was an [O'Reilly book on XPath](http://shop.oreilly.com/product/9780596002916.do)  written more than 10 years ago. Apparently it didn't sell well enough that O'Reilly [released the content](http://commons.oreilly.com/wiki/index.php/XPath_and_XPointer) on its commons site. There are many online sites that documents XPath, though not well enough. I learned what I could.

I started by drafting the chapters I wanted. It is easier to start writing with a framework in mind. From then on, it is just a matter of showing up everyday and churning out the content. [Streaks](https://streaksapp.com/) helped maintain my momentum. I wrote in plain text files on [Emacs](https://www.gnu.org/software/emacs/). I read about [Scrivener](https://www.literatureandlatte.com/scrivener.php) but was never tempted as my fingers speaks Emacs. The text editor doesn't matter as long as content gets written. At first the format of my text was [Markdown](https://en.wikipedia.org/wiki/Markdown). After I learned more about formatting, I switched to [Asciidoc](http://asciidoctor.org/docs/asciidoc-writers-guide/). Asciidoc suits ebooks better.

## Formatting

The ebook had to be formatted in the major formats: [HTML, PDF, MOBI and EPUB](https://en.wikipedia.org/wiki/Comparison_of_e-book_formats). There are several tool chains available. I wanted a tool chain that can be used on the command line. I found [Gitbook](https://www.gitbook.com/) and [Softcover](https://www.softcover.io/). I also saw [O'Reilly Atlas](https://atlas.oreilly.com/). Apparently it is invite only.

It was a toss up between Gitbook and Softcover. I tried Gitbook first as its eco-system looked more mature. The Gitbook workflow fit what I was looking for. I didn't need to try Softcover. Gitbook depends on [Calibre](https://calibre-ebook.com/) for formatting the ebooks. Calibre worked well enough.

## Workflow

I had a [Makefile](https://www.gnu.org/software/make/). `make serve` would call Gitbook's app server which renders Asciidoc or Markdown into HTML. I'm able to view the rendered content immediately. When I make changes, it automatically updates. The feedback loop is fast.

When I needed to build the books, I would call `make build` which calls the individual Gitbook build commands.

```
serve:
	../node_modules/.bin/gitbook serve

pdf:
	../node_modules/.bin/gitbook pdf ./ ./build/everydayxpath.pdf

epub:
	../node_modules/.bin/gitbook epub ./ ./build/everydayxpath.epub

mobi:
	../node_modules/.bin/gitbook mobi ./ ./build/everydayxpath.mobi

build: pdf epub mobi
	echo "built pdf, epub, mobi"

.PHONY: serve
```

That was all I needed.

## Website and payments

The [EverydayXPath](http://www.everydayxpath.com) website is a static HTML site hosted for free on [Github Pages](https://pages.github.com/). The website was written by me, the design was not. I bought a [Bootstrap](https://getbootstrap.com/) theme from [Wrapbootstrap](https://wrapbootstrap.com/). The theme itself is a bunch of static HTML files. I was able to modify from it and get a professional looking website done in very short time, which is about 5-7 hours. I could have written one from scratch, but I would never be able to finish it in 5-7 hours.

When developing the website, I used [http-server](https://www.npmjs.com/package/http-server), a [Node.js](https://nodejs.org/en/) package. It is a light web server which can render my website. I didn't want to install [Nginx](https://www.nginx.com/) or [Apache](https://httpd.apache.org/) for rendering a one page website. That is overkill. http-server suited my needs perfectly.

For delivery of the digital product and payments, I chose [Gumroad](https://gumroad.com/). Gumroad doesn't charge a monthly subscription, which suits me as I didn't know how the ebook would sell. They charge a commission for every sale. If there is consistent volume, Gumroad offers a [subscription](https://gumroad.com/features/pricing) model that has lower commission rates. The __buy__ link on my website leads to Gumroad, where the payment takes place. Gumroad also handles the sending of the ebook to my customers.

## Marketing plan

I know that writing the ebook is perhaps only 20% of the work. The real work is marketing. Hence, I'm putting up all the content from my ebook in a series of blog posts every week. My intention is to give would be readers a taste of the content. Hopefully I can convert them. The blog is an avenue for feedback. I also intend to measure all the marketing initiatives and publish the conclusions here. The world needs more data points on marketing technical ebooks. For a start, I'm going to concentrate on organic marketing techniques. Techniques that doesn't require any money. I'm not ruling out paid marketing. So dear reader, if you're interested in following this journey, do stay in touch by subscribing to this blog. Thanks!
