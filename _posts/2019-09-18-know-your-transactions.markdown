---
layout: post
title:  "Know Your Transactions"
date: 2018-09-18
categories: [microservices, transactions, database]
description: Know Your Transactions.
image: "/assets/img/transactions.jpeg"
image-sm: #https://picsum.photos/200/300/?random

---

A corrupted database is a nightmare. Customers run into unexpected behavior resulting in numerous bugs that are really hard to track down and fix, usually requiring manual tweaking of the database.
<br><br>
When working with centralized systems that have a single database, this is an issue relatively easy to solve: **wrap every business operation with a transaction that either success or fails**. For example, a "user creation" operation might require updating several tables in the database: users, roles, etc. Make sure these updates are all executed within the same transaction. If updating "roles" fails, you can rollback the transaction and leave the database in a consistent state. Beware that transactions lock resources so you'll have to figure what's the best locking strategy to support your concurrency needs. Concurrency is another aspect to working with databases, e.g. optimistic locking: <https://github.com/zalando/restful-api-guidelines/blob/master/chapters/best-practices.adoc#optimistic-locking-in-restful-apis>.
<br><br>
When working with distributed systems, things get complicated. Distributed transactions exist, but they only work in happy scenarios and do not scale well as they lock resources. Let's consider the previous example in a distributed environment with a "user service" and a "role service": creating a user means calling both services, but what do you do if "role service" fails after calling "user service"? You might be able to programatically rollback "user service", but what if "role service" times out? There's no way you can tell whether it actually updated the database, should you still rollback or should you retry? How many times?<sup>[ (1)](#fnOne)</sup>. What about concurrency? Both services are "somewhat locked" meanwhile the distributed transaction is executing; what happens to other calls? There are many edge cases that make distributed transactions hard and it's better to avoid them <sup>[ (2)](#fnTwo)</sup>.
<br><br>
What do you do then? There are a few ways to solve this problem by using an **asynchronous model to communicate between services**: messages, events, etc. In this model a message is sent to both services that will trigger the database update for each of the services. This way, both services will be eventually consistent as messages are processed independently.
<br><br>
Here are few articles I recommend reading to get a better understanding on how this works:<br><br>
-Event driven architecture: <https://www.nginx.com/blog/event-driven-data-management-microservices/><br><br>
-Event sourcing and CQRS <https://medium.com/technology-learning/event-sourcing-and-cqrs-a-look-at-kafka-e0c1b90d17d8><br><br>
-Business events result in a single synchronous commit: <http://www.grahamlea.com/2016/08/distributed-transactions-microservices-icebergs/><br><br>
In some cases, it's not possible to implement these models or the cost doesn't pay off, then it's imperative that these errors are visible as soon as they happen, so the team can quickly act and fix them.
It's not easy, but there's no way around it. Centralized or distributed, there must be a strategy in place to keep the data consistent.
<br><br>

---

<br><br>
<a name="fnOne">(1)</a> There are cases where introducing asynchronous communication is not possible thus a retry strategy must be in place. There are some good libraries for implementing automatic retries and whatnot: Polly for .NET (<https://github.com/App-vNext/Polly> for .NET) and Feign for JAVA (<https://andrewtarry.com/spring_boot_hystrix_feign/>)<br><br>
<a name="fnTwo">(2)</a>	The fallacy of distributed transaction: https://ayende.com/blog/167362/the-fallacy-of-distributed-transactions<br>