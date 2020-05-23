---
layout:     post
title:      "It is never a MVP"
subtitle:   "Postmortem of launching an app"
date:       2020-05-24 12:00:00
author:     "Ja.Herrera"
header-img: "img/posts/post-23052020-memgramm-header.png"
comments: true
tags: [ frontend,Backend, flutter, firebase, golang ]
---

I recently released an app for improving English for Spanish speakers. The app is called [MemGramm](https://play.google.com/store/apps/details?id=com.zancocho.spanglishcards) and it is currently available on Android. It took me a couple of weekends to build the app but a year to finish it. I built it for me and the idea is quite simple and not imaginative at all: English flashcards. However, it has a bit of engineering under the facade.

A brief description, my app offers predefined flashcards divided into different categories and levels of difficulty and the user has to fill the blank space. Also, users can add their flashcards and search by words.

![banner](/img/posts/post-23052020-banner.png "Banner MemGramm")
 
## Motivations

As I said, I built it for me at the beginning. When I moved to London, I used to create notes or write down every word or sentences in English that I saw in everywhere. There are already some apps for flashcards but none of them convinced me.

Also, work with technologies that I don’t usually work. The chosen ones were Flutter for the front-end and Golang for building some internal tools.

And finally, have something to show and how to manage a considerable amount of data with a low-cost architecture.

## Technologies

The back-end is built with Firebase Cloud Functions because it was the faster way for building a back-end and also the cheapest option (basically free). I considered Amazon Lightsail which has a fixed price but free is free. Also, I rely on Firebase to authenticate users.

About the front-end, I picked up Flutter because I wanted to build one and publish for Android and IOS and Flutter looked (and still looks) promising. I could choose React Native but Flutter got my attention.

Finally, I introduced Golang into my stack to build an internal tool for managing the content.

## First version

The first version consisted basically on call to Firebase Functions endpoints (HTTP requests) to retrieve the data and store the data on local storage (SQLite). If the phone didn’t have access to the Internet, the phone would display the data from local storage.

As you can see, the first version wasn’t a big deal, however, an endless number of improvements came out and with that, a lot of learning as well.

### Improving time response

The first version worked fine, however, the more content I added, the API calls took longer. To improve this, I added the last date call as a parameter in the endpoint so the client sends the last date and will receive only the updated data.

```
{
  decks_updated:[],
  decks_id_available:[]
}
```

The client updates only the decks included in decks_updated and deletes any decks in local storage which are not in decks_id_available so we can also enable or disable decks from the backend. 

Finally, if the user swipes to refresh, the client will request the backend for the full content, deletes all the decks from local storage and store the new decks.

![firebase1](/img/posts/post-23052020-memgram-firebase1.png "Firebase request content")

## UX vs over-engineering

As a backend engineer, I wanted to customize the client directly from the backend, so the backend was proving the size of each deck (Card widget in Flutter) and even the backgrounds images for them. 

I was very proud of this, however, there was a bit delay in displaying the images the first time they were updated. Despite my efforts for delivering the customization, the user experience wasn’t great at all, so finally I opted for the simplest solution: have a list of background images in the app so backend only needs to provide the name of the image. 

Now, the app works smoothly but I would need to publish an update if I wanted to add more background images. However, in that case, the app won’t break because it will display a background image by default.

## Content management

Adding decks and sentences one by one into Firebase database was an awful task and also, the client could make a call just when I was updating the content so the content wouldn’t be consistent. For that reason, I needed to develop a tool to help me manage the content.

The content is managed on a Google Spreadsheet and when I am fine with the content, I only need to run the tool which will update my Firebase Database. Although the tool is quite basic, it allows me:

- Create or update specific decks (sheets).
- Enable and disable sentences.
- Check the format of the content is correct in the spreadsheet.

![firebase2](/img/posts/post-23052020-memgram-firebase2.png "Content management")

## Documentation

I wasn’t writing any documentation at all at the beginning because I thought it would delay me from developing the app. However, as I worked on it only in my free time or weekend, sometimes I needed to spend too much time catching up with my latest changes. 

So I started using [Basecamp](https://basecamp.com/) which is quite straightforward and easy to use. I used it to add my To-Dos, upload files and list of resources and manage some events in the calendar. Once the app was finished, I moved the files and list of resources to a repository on Github.

## Feedback

Feedback is quite important if we are aiming for a well-polished app. The feedback helped a lot but if I had to highlight:

- UI and UX design: before feedback, my app looked like an Android app from 2012. Also, my app had a lot of redundant buttons which I removed so the experience is much better.

![feedback](/img/posts/post-23052020-compare.png "Design comparison")

- Content: my app is for Spanish speakers to improve their English however I received feedback about the content was too advanced and I needed to add content more focused on beginners.

## Future improvements

In the short term, my idea is only to add new content (and fix bugs) but I have a list of To-Dos if the app gets successful with a decent amount of active users:

### Engineering 

- Favourites sentences saved on the back-end: I decided not to save them on the back-end to avoid to reach the limit of the free plan of Firebase.

- Pagination in Search tab: the full list of sentences is currently shown on the page. Now it is not a problem but it might be if I continue adding content. 

### New features

- Gamification: thinking in something easy to implement like for example a day streak to keep the user using the app every day.

- Voice translator for every sentence: open Google Translator or Wordreference in an embedded browser, or I could go further and play with AWS Polly or the equivalent on Google Cloud.

### Improvements

- Native ads integrated on the home screen and remove banners: Flutter doesn’t still have proper integration with Admob and also I wasn't able to add Native ads without compromise the release. 

- Improve the internal tool for showing statistics for the plays: the app currently makes a call to Firebase to store the score, level and time after every play so it would be nice to have a visualization of this so it can help me to normalise the levels of each deck.
