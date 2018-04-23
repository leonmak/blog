---
title: "CS3216 Exchange Buddy"
date: 2016-08-15T18:20:28-07:00
draft: true
---

CS3216 is a 'selective' course, and has pushed me to developing my development skills. The following is our process thus far for the exchange buddy web app:

## From Ideas to Paper
-----------------------------------------------------------------

After discussing as a team, we decided to go with the exchange buddy idea. Our first step was to list down the features we thought exchange students, or ourselves, would want. Eugene already had a simple wordpress site he built for exchangebuddy.com, and it was insightful to hear his view on the needs of exchange students, something I myself had not yet considered (being year 2, probably I should). Mainly the gap we identified was that exchange students who do not go to places with their friends do look for others who are going on exchange to the same university with them. Normally there would be a google form where people would fill in their contacts and people could view who else were going to that university, or a facebook group (for example: Singaporeans in Cambridge).

Also issues like accomodation, information or paperwork (for example: rules for visa holders setting up bank account) were some of the common pain points we got from talking to our friends who as of this writing had just left for exchange, which corroborated Eugene’s research from exchange buddy.

## From Paper to Sketch
--------------------------------------------------------------------

We then made simple paper prototypes (or on whiteboard) where we threw around ideas and layouts.

I then made a few mockups thinking of the components we would use. Pictures speak louder than words so:

[![](http://res.cloudinary.com/leonmak/image/upload/v1471907440/Group_Page_Wiki_wzjtdd.png)](http://res.cloudinary.com/leonmak/image/upload/v1471907440/Group_Page_Wiki_wzjtdd.png)

[![](http://res.cloudinary.com/leonmak/image/upload/v1471907440/Group_Page_Chat_s3pwzy.png)](http://res.cloudinary.com/leonmak/image/upload/v1471907440/Group_Page_Chat_s3pwzy.png)

[![](http://res.cloudinary.com/leonmak/image/upload/v1471907441/Tips_Page_hfx4rr.png)](http://res.cloudinary.com/leonmak/image/upload/v1471907441/Tips_Page_hfx4rr.png)

After a few rounds of discussion our features changed, for example we didn’t think a listing of other university exchange groups would be that important. Also the grid layout for tips became more of a list.

[![](http://res.cloudinary.com/leonmak/image/upload/v1471907440/Group_Info_uoajdk.png)](http://res.cloudinary.com/leonmak/image/upload/v1471907440/Group_Info_uoajdk.png)

[![](http://res.cloudinary.com/leonmak/image/upload/v1471907461/Group_Chat_apm2pc.png)](http://res.cloudinary.com/leonmak/image/upload/v1471907461/Group_Chat_apm2pc.png)

[![](http://res.cloudinary.com/leonmak/image/upload/v1471907462/Group_News_wfldyj.png)](http://res.cloudinary.com/leonmak/image/upload/v1471907462/Group_News_wfldyj.png)

[![](http://res.cloudinary.com/leonmak/image/upload/v1471907470/Group_Info_rrz7u0.png)](http://res.cloudinary.com/leonmak/image/upload/v1471907470/Group_Info_rrz7u0.png)

I’ve been using Sketch for a while after my photoshop had expired and I actually rarely had to go back unless I’m working with heavy textures for which Sketch being vector-based is not the best for. But I’m quite biased toward it because I feel it’s less bloated and hangs less on my Macbook Air. Also I can easily export images without having to scale the artboard beforehand so it’s always so crisp, a plus for viewing/ printing. There are also resources like [sketchappsources.com](http://sketchappsources.com) where many talented designers post their work, much respect to them!

## From Sketch to React
--------------------------------------------------------------------

To build our prototype for the mid-submission, we relied on libraries like material-ui to provide components like cards that we could use, which by default looks ‘better’ than bootstrap. After getting some feedback from tutors and students we also decided that news itself would be quite irrelevant and local events would suit the students need better.

Other React components we needed were also available which meant Irvin could also focus on what I saw as the hard part which was the backend where we were going for ‘Reactive MySQL’, ie. using meteor’s pub sub features with MySQL as our database for real-time updates. Meanwhile I just mocked the data although Thanh and Irvin had made a schema it made developing easier without having to worry about the backend for now.

So as of mid-submission we got most of the layout up and it looks as such:  

[![](http://res.cloudinary.com/leonmak/image/upload/v1471912017/Screenshot_2016-08-23_08.26.08_jptakn.png)](http://res.cloudinary.com/leonmak/image/upload/v1471912017/Screenshot_2016-08-23_08.26.08_jptakn.png)

[![](http://res.cloudinary.com/leonmak/image/upload/v1471911401/screencapture-exchangebuddy-irvinlim-group-chat-1471907721545_qr13pk.png)](http://res.cloudinary.com/leonmak/image/upload/v1471911401/screencapture-exchangebuddy-irvinlim-group-chat-1471907721545_qr13pk.png)

[![](http://res.cloudinary.com/leonmak/image/upload/v1471911402/screencapture-exchangebuddy-irvinlim-group-events-1471907730288_hmu7cz.png)](http://res.cloudinary.com/leonmak/image/upload/v1471911402/screencapture-exchangebuddy-irvinlim-group-events-1471907730288_hmu7cz.png)

## Draw it again
-----------------------------------------------

So after building the rough layout we decided to change the info tab and avatar menu as we felt that it didn’t really work out when we were building it. For example the list of cards were really not feasible if there were so many cards and each would be so big when expanded. We decided then to split it into a home tab and an info tab where each grid section linked to it’s own article, so it was more of the original grid layout. I felt that although some more work had to be done to reorder the layouts it was a necessary decision we had to make. So back to the drawing board for those parts: