---
layout: post
title:  "Your domain entities are not your microservices"
date: 2018-06-15
categories: [DDD, microservices]
description: Your domain entities are not your microservices. The contexts where they exist, are.
image: "/assets/img/trap.jpeg"
image-sm: #https://picsum.photos/200/300/?random

---
Here's a company I follow closely as I believe we at Midaxo are on the same path they were a couple of years ago when moving from a centralized to a distributed architecture: <https://www.smartly.io/blog/highlights-from-devtalks-microservices-and-advanced-sql>
<br><br>
Building a distributed architecture is like everything else in software development: a continuous and iterative process. You start by splitting the centralized architecture into a few services driven by the needs: the business (product roadmap), technical (scalability, maintainability, etc.) and team (people). After a while, as your product grows and changes, you continue the process of splitting the services into more and more services based on the same needs.
<br><br>
The main goal of the architecture is to support the business and since it can be tricky at times to anticipate those areas that the business will grow in the future, it makes more sense to focus on the immediate future, the now, and split the architecture to address those needs that we have presently.
<br><br>
**Your domain entities are not your microservices** - A trap where you can easily fall if you don't give careful consideration to the boundaries of your services, which is where Domain Driven Design and bounded contexts come handy. In the example mentioned in the blog, the author didn't build three microservices: "flights", "passengers" and "crew" (the entities), but rather "passenger flights" and "crew flights" (the contexts where the entities exist). If the author would have built a service per entity, "passengers" and "crew" would be bound to "flights" and lose their autonomy. One way to look at it, is that flights only exist in the context of passengers and crew, since there's no flight without the crew or the passengers, so there's no reason to model it as a stand-alone service.
<br><br>
Therefore, thorough thinking is what's needed when deciding the services' boundaries. And it's important to keep in mind that you don't have to build it all at once, nor get it right but rather build enough to support the needs that both your product and teams have right now.
<br><br>

---

<br><br>
Domain Driven Design, Command Query Responsibility Segregation and Event Sourcing are design patterns used by the developer community to help finding the boundaries of every service. The DDD is purely based on the business needs, CQRS is about decoupling or loosely coupling components and ES is about dealing with the application state.