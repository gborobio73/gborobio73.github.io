---
layout: post
title: "Shared Shelves"
date: 2016-08-01
categories:
  ['books', 'software-development', 'google-cloud']
description:
image: "/assets/img/bookshelves.jpg"
image-sm: #https://picsum.photos/200/300/?random
---

During the fall 2013, I wanted to introduce AngularJS to the company I was working for. To be able to do that I had to learn it first. Around the same time, my wife and me moved to a new apartment, which made us realize that we had too many books.

That's the reason why I built **[Shared Shelves](http://www.sharedshelves.net/)**.

You can find more details about the app [here](http://www.sharedshelves.net/#/FAQ).

## Technical Details:

It's a SPA with a Java backend, all hosted in Google cloud. 

**Frontend**

- HTML, CSS, JavaScript and [AngularJS 1.4](https://angularjs.org)

**Backend** 	
	
- Java 7 / Google SDK 1.9.5

- [Objectify](https://github.com/objectify/objectify) and [Google Cloud Datastore](https://cloud.google.com/appengine/docs/java/datastore/) 

- [Google Books](https://books.google.com) engine, plus Finnish and Spanish online bookstores.

Frontend and backend communicate via RESTful interfaces. There are also servlets that deal with log in/out.

I'm still developing it, but not as actively as my other pet project: [Tennis Score for Pebble](https://gborobio73.github.io/2016/10/03/tennis-score-for-pebble/)

Wanna see the [code](https://github.com/gborobio73/tbe)?

