---
layout: post
title: "Using more than a hundred stored procedures in production"
date: 2017-04-23 06:50
comments: true
categories:
---

At my present company [Courex](https://www.storeviva.com/), we make heavy use of stored procedures, triggers and functions to run our production applications for over 2 years. At present, we have 258 stored procedures, 107 functions and triggers. Few companies use this many stored procedures. Would we recommend it? Yes.

<!-- more -->

Life started as a conventional PHP-MySQL web application built by an outsourced team. Obviously with outsourced teams hired on a shoestring budget, it was plagued with performance issues and bugs.

## Good: Abstraction

The PHP code was taking input from a form, constructing SQL queries, running them, finally rendering the results. There were a series of different queries to run. This pattern repeated in most parts of the application. Thus the idea of using stored procedures was mooted. What stored procedures did was it abstracted over the series of queries as one procedure call, simplifying the code.

In the past the code did this

``` sql
SELECT x, y, z FROM foo;
SELECT a, b, c FROM bar;
UPDATE foo SET i = j WHERE baz = boo;
```

Now we do this

``` sql
CALL update_foo(x, y, z);
```

They are no different from functions. All the usual engineering best practices apply. For example, we are able to name the stored procedure. Now we can use a meaningful name called `create_delivery` and it does what we expect - insert a delivery row. Instead of updating the same query repeated across 10 files, we update the one stored procedure. That's a maintenance win.

## Good: Code reuse

We have code bases in different programming languages, but 1 database. All code bases have access to those same functions and procedures on the database. We have procedures that converts types, procedures that does validations, triggers that updates time stamps. All that comes free as long as you can make a MySQL query.

In fact, we implemented our message queue purely in stored procedures. All our applications were able to _just_ use it without much fanfare since it is just a sql query.

## Good: Well understood

SQL along with Javascript are basic skills that we came to expect from every developer. From our experience, introducing them to our team and learning to use them is a non-issue. Jumping in to maintain them is also trivial. That's a win.

## Good: No extra dependencies, stability and performance

The best thing is there are no extra dependencies. I've came to appreciate projects with less moving parts. The less dependencies there are, the easier it is to setup and maintain.

MySQL also does not introduce major versions with breaking changes every few years. Using a mature relational database, we enjoy a level of stability and performance we don't usually experience from other software. We enjoy the peace of mind it brings.

Of course there is a flip side.

## Bad: SQL has its warts

SQL is not a good language for writing complex logic. Its warts slows down development time. Take error messages for example.

```
SQL Error (1064): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'foo' at line 1 */
```

We frequently encounter error messages that does not help. Non-helpful error messages slow down development.

Which brings me to our next point - readability. SQL is hardly readable when queries gets long. Being free-format led to a Cambrian explosion of indentation styles in the wild. I do not relish debugging a three hundred line query with non-helpful error messages.

I firmly belong to the camp where the more restrictive the language is, the easier it is to write correct code. SQL, without the strict mode, is too forgiving.

```
SELECT "1" + "1" // returns 2
SELECT 1 + "1"   // returns 2
SELECT "a" + "a" // returns 0
SELECT 1 + NULL  // returns NUL
```

Even when expressions does not make sense, it returns something. It should not be type casting automatically, worse, not even showing a warning, giving a false sense of security. This can lead to dangerous bugs. Imagine the query is trying to add a sum of money to a bank account, and somehow one of the parameter is `NULL` or an alphabet, nothing will get added. As SQL deals directly with data, the impact of mistakes gets magnified.

## Bad: It needs powerful hardware

As we use hundreds of stored procedures and functions, it is highly demanding on resources. Our database server has to be powerful. We're forced to use dedicated hardware. In fact we tried using AWS RDS's `db.m4.4xlarge` which is a 16-core 64GB RAM virtual machine with the max IOPS allocated. It didn't work. We encountered table locks every few hours. We've tried various ways to tune it, but the locks did not go away. Mysteriously, once we were on dedicated hardware, it went away. Granted there could be ways to solve it on RDS(we aren't professional DBAs), but it showed us that we need to invest on powerful servers for this approach to work.

There is obviously the problem of vendor lock in. SQL is not portable between the different databases. But isn't that the same for your `<insert your language>` code base? To me this is a non-issue. I've also heard cases against stored procedures because of deployment or versioning. I'm not even sure they are problems to begin with. Treat stored procedures like normal code. Use a version control system. There is a tool [Flyway](https://flywaydb.org/) which helps. Things you expect in modern deployment such as versioning, rolling back are all taken care of by Flyway.

Why wasn't stored procedures more popular? I suspect it is because it is not _new_ and not _marketed_ much if at all. Most times, it is a short chapter on a SQL book. There are always shiny new things to adopt. Stored procedures does not have the _cool_ factor. But it is [stable, boring and it works](http://mcfunley.com/choose-boring-technology). I got to thank my colleague [Pierre](https://github.com/prenaux) who instigated this move. Hopefully more people would come to appreciate this feature of relational databases that almost no one talks about.
