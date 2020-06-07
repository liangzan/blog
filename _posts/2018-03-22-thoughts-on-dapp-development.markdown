---
layout: post
title: "Thoughts on dApp development"
date: 2018-03-22 17:07
comments: true
categories:
---

I've recently got into [dApp](https://blockgeeks.com/guides/dapps/) development. Having programmed for a while, here're some thoughts on building dApps on [Ethereum](https://www.ethereum.org/).

<!-- more -->

## Smart contracts are powerful

Interacting with [Smart Contracts](https://blockgeeks.com/guides/smart-contracts/) feels like interacting with an instance of an object. It feels familiar yet odd at the same time. The familiar bit comes from the interaction. You can call functions on it, you can store value on it, it is like regular programming. The weird bit is this is happening in **public**. Every transaction can be held to scrutiny and there is no argument whether something happened or not. That is powerful.

Smart contracts is a novel angle of encapsulating logic and data. Immediately I can see that it solves the payment problem for the couriers. The customer can put out a smart contract, put ether in it and wait for couriers to participate. Place delivery jobs in a variable, let couriers can bid publicly via transactions, and pay them by the contract after they perform the delivery successfully. There would be no basis for disputes. Today, such logic is contained in monolithic applications controlled by different logistics companies, and they don't inter-operate well. I can imagine a future where developers will primarily write smart contracts, and leave the rest to the blockchain infrastructure.

## dApps development is not hard

I've read about the shortage of blockchain developers. This led me to assume that dApp development is hard. Frankly, I don't find its learning curve steeper than a new programming language or a new Javascript framework. In fact I find learning [Scala](https://www.scala-lang.org/) or [React](https://reactjs.org/)/[Redux](https://redux.js.org) harder. It's suffering from growing pains where documentation and tooling support are lagging behind. I also find that you need a basic understanding of how blockchains work. Otherwise the borrowed metaphors such as wallets, addresses, transactions would be confusing. On the whole, things do work, albeit with some rough edges.

For example, [Ganache](http://truffleframework.com/ganache/) would crash sometimes. For me, that happens once every other day; annoying but livable. I was old enough to have used [Windows 98](https://en.wikipedia.org/wiki/Windows_98); that crashed every 2-3 hours. No doubt in 2018 we should demand higher standards. Another example - [Truffle](http://truffleframework.com/). Again documentation cannot catch up. I wanted to deploy contracts using Truffle and I found no documentation. In the end, I had to dig through the tests of [truffle-deployer](https://github.com/trufflesuite/truffle-deployer) to learn how to apply it.

Don't get me started on [Solidity](https://solidity.readthedocs.io/en/v0.4.21/). I was trying to do something basic - transferring funds to an address. I thought this _ought_ to be a solved problem since transactions are what Ethereum is meant for. Instead I found an intuitively named function called `call` used for that purpose. Flabbergasted, I checked the documentation, where I found two functions named `transfer` and `send` written for that purpose. Why isn't people using that? Could programmers are trying to be clever? When the source code is often the only source of documentation, this leaves me feeling apprehensive. Luckily after digging deeper, I realised that `send` fails _silently_ when the recipient runs out of gas, hardly something uncommon. If I'd like to interact with a contract, how do I know what functions are there? Sadly there is often no documentation; there is _only_ Solidity code. The onus is on the developer to understand what the contract does by reading the code. Sounds like Haskell. Another annoyance is Solidity errors are often not meaningful. I was in the dark as to what happened when the contract crashed. After much searching, I realised I have to use the [debugger](http://truffleframework.com/docs/getting\_started/debugging). The debugger worked, thankfully. The worst bit is the bug came from calling a non-existent constructor. How did a wrongly named constructor pass the compiler? One last thing - Solidity doesn't have syntax highlighting on _Github_. Programming with the blockchain is like programming in the 80s - not for the faint of heart.

Blockchain is like the Internet in the 90s. I do see its potential. Its effect would be similar to what containers did for shipping. This time the impact would reach everywhere. Contrary to the norm, I don't find the learning curve for Blockchain development steep. It reminds me of the time when mobile app development was expensive as developers were scarce. Prices came down when more developers learns the craft and enter the pool. It's exactly the same now for blockchain development. Given time, things will improve. If you're still on the fence, don't hesitate anymore, it is time to get started.
