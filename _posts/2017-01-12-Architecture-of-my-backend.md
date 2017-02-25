---
layout:     post
title:      "Architecture of my backend"
subtitle:   "Explaining my architecture and the reasons of my design."
date:       2017-01-12 23:27:00
author:     "Ja.Herrera"
header-img: "img/posts/post-12012017-backend-header.png"
comments: true
tags: [ Backend, pet-project ]
---

## Architecture of my backend 

Currently I´m involved in a personal project, working on the server side and on the client app. Now I´ll focus on the server side and I’ll explain my architecture and the reasons of my design.
First of all, I would like to introduce my product without giving specific information. It´s a mobile app, a mix of social network and game and that in a second version the app will interact with LOT devices.

Two main reasons of my design for the architecture of the server:

1. Scalable: as personal project, I cannot afford expensive servers and deploy lots of micro-services so the architecture is monolithic, but with good practices I'll be able to split it into several applications and maybe into a set of micro-services for some specific feature.

2. Learn new technologies: It´s a long term project, so one of the reasons is learn new technologies. But I´m not a fool, I have more experience with Java (and also I like Java) so I have to take advantage of that. Therefore the core will be write on JavaEE.

![model-backend](/img/posts/post-12012017-backend.png "model of my backend")

## Core

I chose JavaEE–Java 8 because I feel comfortable with Java and along with a Full-application-server Wildfly 10 provides robustness, scalability and security difficult to achieve with other technologies. Initially it´s heavier than other options but luckily we have at our disposal AWS or Google Cloud.

## Database

In my experience building back-ends for apps, the requests from app to database are about 90% read/write and 10% updates. The heavier updates are done by “jobs”, for example a massive expiration of premium accounts. A NoSql database as MongoDB is faster than sql databases in read/write and also MongoDB is based on documents so it’s easier to make changes in our model and design.


## Real time messages

Users will be able to send messages in real time. For that, we will use a protocol based on sockets. I’m still hesitating between using Netty (Java) or Go language. I have little experience with Netty but it’s a safe bet. On the other hand I don’t have experience with Go but it attracts me a lot and this could be a good occasion to give it a try. 

Also, I would like to comment that reject the option of Scala. I have been learning Scala on my own for around one year and I love it but I feel I’m not ready and I need too much time to improve my skills and this is a bit frustrating. Maybe if I had Scala programmers in my environment, I would have encouraged choose Scala.
