---
layout:     post
title:      "No country for old men"
subtitle:   "Embracing new technologies."
date:       2018-10-14 17:27:00
author:     "Ja.Herrera"
header-img: "img/posts/post-14102018-tools-header.jpg"
comments: true
tags: [ pet-project, technologies, spring-boot, intellij, vs-code, vuejs, react-native ]
---

It has been several years since I started my project however, it has been stopped for months because of my new job. During all this time, I’ve been learning new stuff and gaining more experience. 

## Out of the box

I always said we have to know how everything works below the code, dependencies, connection pools, etc so, for this reason, I refused to use frameworks and I kept building my project with Java vanilla. Also the performance, my common sense was telling me that adding frameworks means more code and therefore it affects the performance. 

I’ve been using `Dropwizard` during the last months for some projects. Dropwizard is a framework to build RESTful and allow generate a simply package .jar with all the project and the light-weight Jetty HTTP library. Yes, like Spring Boot, but the most interesting characteristic of Dropwizard is that it’s very easy to convert a JavaEE project into a modern application and provide out-of-the-box support so, for example, we can “dockerize” it in a few minutes.

With this in mind, I started to have a look `Spring Boot` and playing around to see if I could get my project to work and I ended up rebuilding it from scratch. I chose Spring Boot instead of Dropwizard because it looks like it has more support and documentation on the Internet. So now my project is built with Spring Boot and Gradle, even I use the Spring dependencies for MongoDB. Honestly, I’ve not compared the performance between Java vanilla + Wildfly and Spring Boot but now the code has become clearer, It’s easier to debug and I deploy it into a Docker container in a few seconds!.

## Front-end technologies

I already did an internal dashboard to manage the content and it is built with AngularJS 1.6. I would like to rebuild it with VueJs or React but for now, it does the job for a MVP. Also, I prefer to spend my time and focus on the client app.

## App Native or Native?

I was working on an Android app (I barely started it) because I have some experience developing Android apps. However, since I am living in London and also in my new job, I realised iOS is really important. In fact, If I don’t have a iOS version, maybe I won’t be able to find enough people to try my MVP.

I am not interested in learning Swift and I don’t have enough time for learning a new programming language from zero. Also, I don’t want to look for an iOS developer (at least not yet) because for now, it is a personal project which I work in my free time. So I have to go through another alternative. I developed some apps with Ionic in the past and I think it doesn’t fit for my project, also I was very disappointed with the performance. Therefore the only options I have are `React Native` (I already played around it) and `VueJs` (maybe with Nativescript). Honestly, I am more excited about VueJs, mainly because of all the polemic with the React license and Facebook. On the other hand, it is a first MVP so I am not sure VueJs is enough mature so I can develop a fast MVP version with React Native and after that, if people showed enough interest in it, I would look for an iOS & Android developer.

## Tools

I’ve been using Eclipse since my first days working as professional and I felt very comfortable with it: I had my setup, plugins, shortcuts,... my specific version which I was scared of update it just in case the update broke my local environment. I tried `IntelliJ (Community edition)` and I am not scared anymore of click on install updates. Also, my laptop doesn’t look like it is going to take off from the table.

Also, sadly I replaced Atom with `VS Code` because the same reason than Eclipse, Atom takes a lot of resources from my laptop. I am not sure with this decision because I really like Atom so I hope to come back after a new release.
