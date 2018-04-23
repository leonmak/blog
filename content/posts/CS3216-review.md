---
title: "CS3216 Review"
date: 2016-11-25T19:14:43-07:00
draft: true
---

I thought of doing a more specific breakdown of the web applications I had contributed to. Hopefully this can also give people (like my juniors) who want to take this module an idea of what to expect, but really the type of ideas are limited by your creativity. There are also some groups who did native for final project, but it’s because they are (really) good at it. I’ll not be covering Assignment 2 as it was an application critique and no building was done. If you’re rreallyy ‘kiasu’ (Singlish for: scared to lose) also check out the [CS3216 website](http://cs3216.com/coursework).

## Assignment 1: Facebook application - [Exchange Buddy](http://app.exchangebuddy.com)

### Description:
Web application for exchangebuddy.com started by Eugene to connect people going for exchange programs. Parts of Facebook API used: facebook login, facebook events API. We received the highest ‘coolness’ factor points but we got about average overall mainly due to our report being rushed, and thus not answering some of the questions properly.

### Features:
*   Grouping exchange students by their exchange batch
*   Real-time chat for exchange group
*   Events lookup (using facebook and meetup.com API)
*   Collaborative editing of tips

### Stack:
*   Front-end: React, MeteorJS (for real-time pub-sub)
*   Back-end: MySQL
*   Hosting: Digital Ocean (frontend), Irvin’s VPS (backend)

## Assignment 3: Progressive Web App - [Drop Ins](https://dropins.space/)

### Description:
Realtime chat application where you can drop emoticon pins on a pokémon GO themed map, supporting videos and soundcloud links. Got above average for this but I thought we could have polished it a bit more if we had more time. Uses facebook login.

### Features:

*   Real time updates of ‘drops’, votes, comments using socket.io
*   3D Vector Map view using Mapbox GL
*   Offline caching with sw-* packages

### Stack:
*   Front-end: React (using [Create-React-App](https://github.com/facebookincubator/create-react-app/) boilerplate)
*   Back-end: MySQL
*   Hosting: Heroku (frontend), AWS RDS (SQL server)

## Final Project - [1our](https://1our.today)
### Description:
1our is a platform for NUS students maximize their time in school, and get paid for it. We do this by bringing them fun and interesting projects, studies as well as experiments that they can be part of. 1our also wants to help NUS student researchers and professors succeed in whatever they are doing, be it a study on decision-making behavior or an experiment pushing the frontiers of science.

### Features:
*   NUS OPENID login
*   Time slots for each ad-hoc jobs
*   Tracking of money earned through platform
*   Listing of adhoc jobs by category with sorting and filtering

### Stack:
*   Front-end: React, Webpack
*   Back-end: PostgreSQL
*   Hosting: AWS EC2 (frontend), AWS RDS (backend)
