---
layout: post
title: "Prefer lifting to destructuring in Scala"
date: 2016-03-13 15:56
comments: true
categories:
---

This is one of the few things which I discovered along the way as I was learning Scala. Always prefer lifting a function than destructuring.

<!-- more -->

For example, I have a function which returns the capital given the two letter country code.

``` scala
def countryCapital(countryCode: String): Option[String] = ???
```

This function returns an `Option[String]` type. If the capital exists, it returns the capital as a `String` contained in the `Option` monad. If not, it returns a `None`.

I want to upper case the results returned. This was what I was doing in my early days.

``` scala
val capital = countryCapital(countryCode)
val upcasedCapital = capital match {
   case Some(s) => s.toUpperCase
   case None => None
}
```

Then I realised lifting the `Option` type is a more idiomatic way.

``` scala
val capital = countryCapital(countryCode)
val upcasedCapital = capital.map { c => c.toUpperCase }
```

It is not necessary to destructure the type only to put it back again. Look how short it is, and clear its intention is. It did exactly the same things the earlier code snippet did.

When you see a lot of destructuring using pattern matching in a Scala code, it is almost always a code smell. Always prefer lifting over destructuring.
