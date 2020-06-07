---
layout: post
title: "Everday XPath - The Expression"
date: 2016-09-11 05:53
comments: true
categories:
---

This blog post is part of a series on XPath. The content comes from my ebook [EverydayXPath](http://www.everydayxpath.com). Part of the content from the book will be released to the public as blog posts. In this post, we explain what XPath does. We disect the components of an XPath expression. And why the context is the key to forming the expression.

<!-- more -->

## The Expression

When we travel to a foreign country, we rely on directions to get to the place that we want. Similarly, an XPath expression is like us giving the computer directions to find a particular node in the document.

An XPath expression is made up of steps, evaluated from left to right. Each step moves closer to the target element. They are separated by the forward slash `/`.

```
step/step/step
```

We define a step as a *location path*. A location path selects a set of nodes relative to the preceding step. When a location path starts with a forward slash, it starts from the root node.

A location path can be broken up into 3 distinct parts. An axis, a node test and a predicate. It follows the format below. Only the node test is required in an expression. The rest are optional.

```
axis::node-test[predicate]
```

The __axis__ makes use of the relationship between the nodes selected to the context node to act as a filter. For example, the ancestor axis selects the ancestors of the current context. While the descendant axis selects the descendants of the current context. The axis is optional.

The __node test__ can be thought of as a pattern used for matching the desired nodes. We make use of the properties of the node to form the pattern. For example, a valid node test would be `table` which means we want table nodes. The node test is required.

The __predicate__ is a filter expression. It filters against the nodes selected in the axis and node test. It evaluates to return either a number or a Boolean value. If it is a number, it selects the node in that position. If it is a Boolean, it selects nodes that satisfies the predicate. The predicate is optional.

## Context

Writing an XPath expression starts with knowing the context. Assuming there is a robot whose job is to fetch you things in the house. The robot understands XPath. I'm in my living room, sitting on the couch.

> From the living room, go to the second room, find the first shelve, bring me the first book.

```
./room[2]/shelve[1]/book[1]
```

The living room was the context from which the expression started.

Assuming the root of the house starts from the door.

> From the door, find the first room, find the first shelve, find the first book

```
/door/room[1]/shelve/book[1]
```

The context was from the root. That is like searching the document from the start.

Since I want a book, if I know the title, I can do this

> Find a book with the title "Infinite Jest"

```
//books[title="Infinite Jest"]
```

The expression changes according to the context. The context sets the basis for the XPath expression. Think of how to choose your context - the expression will turn out differently.

Naturally, the next question is how should we choose the context? While they all work, there are differences in maintainability.

Consider these two factors:

 * Does the page change often? Will your expression break when there are changes?
 * Do you understand what the expression means at first glance?

Avoid using a context which has a high chance of changing. You don't want your expression to break. When writing expressions, always prefer short to long, simple to complex, less syntax to more. Readability helps. Hopefully, these factors will clarify the approach you take when forming the XPath expression.
