---
layout:     post
title:      "My approach to managing log-in"
subtitle:   "Let’s go ahead without any help"
date:       2017-02-25 13:30:00
author:     "Ja.Herrera"
header-img: "img/posts/post-25022017-login-header.png"
comments: true
tags: [ Backend, pet-project, login ]
---

## My approach to managing login 

Currently I’m working on the log-in flow of my app. At first, I thought to use Firebase because it´s a fast and good solution to manage the log-in ( I used it in small projects) but I think it´s a little limited and also I would be using a third party service when I want to build the entire infrastructure by myself. So let’s go ahead with my approach:

![model-backend](/img/posts/post-25022017-login.png "flow of my login")

I have clear that I do not want to force users to register because that is a barrier for users who just want to take a look the app. Although obviously I prefer that users register. If they don´t want to register (1), their progress will be saved to the local database but they won’t be able to recover your account if you delete the app. Therefore, occasionally a message will be displayed advising them to register so that they don’t lose their progress.

About the passwords, I think the passwords don’t make sense in an app because the users only log-in the first time and I don‘t want to force them to have to think a password, which surely it won’t be safe. To do this, when the user enter the email, a self-generated code (2) will be sent and  the user will only have to be put it the first time in a new installation to validate the email and his account will be associated with the validated email and the device id. A new installation will require to log-in and enter the new code.
