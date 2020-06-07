---
layout: post
title: "Thoughts on React with Redux"
date: 2018-06-01 17:25
comments: true
categories:
---

I have been writing [React](https://reactjs.org/) and [Redux](https://redux.js.org/) for the past months. React and Redux are not the first Javascript frameworks I've learnt; nor is it likely to be the last. We want [something proven](https://blog.liangzan.net/blog/2018/01/12/things-i-would-do-at-a-new-startup/) and something that has a thriving eco-system surrounding it. At this moment(2018), we see 2 options - React or [Angular](https://angularjs.org/). We decided that React with the potential to support mobile apps would be the sensible decision. React coupled with Redux gives us the structure for a modern Javascript application. Well, nothing is perfect. Let's visit some of those aspects where I felt it ticked and where it didn't for React with Redux.

<!-- more -->

## What ticks

State changes in Redux follows a [unidirectional flow](https://redux.js.org/basics/data-flow). It is easier to reason about state when changes are standardised. The benefit becomes apparant when there are more than 20 components. State is found where they ought to be. Coupled with the Redux and React Chrome extensions, debugging is efficient. New hires who know Redux will be productive immediately. That is a hallmark of good design. Good design is intuitive. Having conventions gives productivity gains. In projects where there is no convention for how state flows, wasting time finding where the state is, is inevitable. Redux has achieved its original goal.

It is obvious that Redux is heavily influenced by [Haskell](https://www.haskell.org/). Side effects are kept compartmentalised in actions; similar to the IO monad. The idea of pure functions is borrowed from functional programming. Actions are similar to a Haskell Type. It's heartening to see Redux applying proven ideas from functional programming. The same techniques will serve to keep complexity at bay as the application grows.

## What didn't tick

There are caveats though. The benefits comes when developers follow _the Redux way_ religiously. Redux doesn't prevent you from mucking things up. You can add a pubsub library or a data binding library; Redux won't complain. Staying disciplined and following the Redux way is an exercise in how well ran your team is. There are times when it felt more natural to use pubsub to handle state. In a small to medium sized code base with small teams of 1 to 3 engineers, I'd argue that it makes sense to derive the flow of the state according to what the application does, rather than forcibly fit it to a standard unidirectional flow. For a large codebase with a large team, keeping it standard prevents the application from getting too complex. It is easier for everyone to understand the codebase, helps in debugging and makes it faster to add features.

Another caveat is Redux assumes state is a key-value structure. Often we'd have a list of options in a dropdown where the user selects different options. There are no helpers for handling state in lists. We need to manually manage state changes within the lists, which I thought a mature framework like Redux would have it covered.

Another common grievance - [boilerplate](https://en.wikipedia.org/wiki/Boilerplate\_code). There are lots of boiler plate to write. We have to write the components, the containers, the actions, and the reducers for the most basic of functionalities. I thought this is a design choice. Redux could choose to generate those boilerplate(like [Rails](https://rubyonrails.org/)), or let developers wire these parts on their own. To generate the boilerplate, Redux needs to make assumptions. For every action, which reducer should handle it? Should Redux generate a corresponding reducer for every action? In cases where you want multiple reducers to handle the same action, how do you provide an abstraction for that? How would the [DSL](https://en.wikipedia.org/wiki/Domain-specific\_language) look? Should Redux make such decisions for you? It quickly becomes apparant that abstracting away the boiler plate reduces the flexibility. That's why the having boilerplate is a design trade-off. You write more code but you have the flexibility to wire the components to do what you want.

Almost like the [Maslow's hierarchy of needs](https://en.wikipedia.org/wiki/Maslow%27s\_hierarchy\_of\_needs), every library has its purpose. At the base level, the library has to first work. At the next level, the library would need to work well with others. At the highest level, you'd ask if there is a philosophy guiding its design. A library is an opinion on how to solve a problem. Redux is one library that not only works, it is one that has a strong principles guiding it. It believed in being minimalist, in convention over configuration, in keeping side effects in control, in that you know best and in simplicity. I'm glad to have spent time learning _yet another_ Javascript framework, and I hope I've convinced you too.
