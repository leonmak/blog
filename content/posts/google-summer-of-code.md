---
title: "Google Summer of Code: Loklak"
date: 2016-05-21T17:45:53-07:00
draft: true
---


# Google Summer of Code: Loklak
GSoC [](https://summerofcode.withgoogle.com/) is a program that encourages open source. The project Loklak is under the FOSSASIA organisation, and is a distributed scraper. The following is my proposal for GSoC.

## Abstract
Currently, [Loklak.net](http://loklak.net) has it’s own search and social wall capabilities.
[Loklak.org](http://loklak.org/apps/) also has a few front-end apps.
However, they are currently not as feature filled as [Loklak.net](http://loklak.net), so the project aims to add additional visualisations and display options, including a live-streaming tweet wall, and timeline for loklak.org

## The project

A front-end app that uses loklak.org for API calls to obtain tweets in json format. Users can deploy their own loklak server or locally or online. I have previously developed a prototype with meteor.js [(github repo)](https://github.com/leonmak/meteor-twitterwall) which you can deploy locally or online too!  
![](https://raw.githubusercontent.com/leonmak/meteor-twitterwall/master/public/ss.gif)

But upon discussion with the loklak team we concluded it was hard to integrate with loklak.org like the current front-end apps, so I decided to make the front-end app with a front-end framework like Angular or React.

This is a sample of part of the search.json API response and fields we could use for visualisations:  

```js
{  
 ... ,  
 "search_metadata": {  
 "itemsPerPage": "100",  
 "count": "18",  
 "count\_twitter\_all": 0,  
 "count\_twitter\_new": 18,  
 "count_backend": 0,  
 "count_cache": 17,  
 "hits": 18,  
 "period": 54379295,  
 "query": "from:leonmakk",  
 "client": "118.201.140.222",  
 "time": 1728,  
 "servicereduction": "false",  
 "scraperInfo": "http://kaskelix.de:9000,local"  
 },  
 "statuses": \[  
 {  
 "timestamp": "2016-05-17T05:03:07.163Z",  
 "created_at": "2016-05-17T04:59:47.000Z",  
 "screen_name": "leonmakk",  
 "text": "http://en.wikipedia.org/wiki/PC\_Tools\_(Central\_Point\_Software)",  
 "link": "https://twitter.com/leonmakk/status/732435424145227776",  
 "id_str": "732435424145227776",  
 "source_type": "TWITTER",  
 "provider_type": "SCRAPED",  
 "retweet_count": 0,  
 "favourites_count": 0,  
 "images": \[\],  
 "images_count": 0,  
 "audio": \[\],  
 "audio_count": 0,  
 "videos": \[\],  
 "videos_count": 0,  
 "place_name": "Central Point",  
 "place_id": "",  
 "place_context": "FROM",  
 "place_country": "United States",  
 "place\_country\_code": "US",  
 "place\_country\_center": \[  
 -83.27110293007973,  
 35.6452903550329  
 \],  
 "location_point": \[  
 -122.91642774963586,  
 42.37596116139363  
 \],  
 "location_radius": 0,  
 "location_mark": \[  
 -122.91596190504673,  
 42.37590151256903  
 \],  
 "location_source": "PLACE",  
 "hosts": \["en.wikipedia.org"\],  
 "hosts_count": 1,  
 "links": \["http://en.wikipedia.org/wiki/PC\_Tools\_(Central\_Point\_Software)"\],  
 "links_count": 1,  
 "mentions": \[\],  
 "mentions_count": 0,  
 "hashtags": \[\],  
 "hashtags_count": 0,  
 "without\_l\_len": 1,  
 "without\_lu\_len": 1,  
 "without\_luh\_len": 1,  
 "user": {  
 "appearance_first": "2016-05-15T06:28:23.263Z",  
 "profile\_image\_url\_https": "https://abs.twimg.com/sticky/default\_profile\_images/default\_profile\_3\_bigger.png",  
 "screen_name": "leonmakk",  
 "user_id": "728557565676642306",  
 "name": "leon",  
 "appearance_latest": "2016-05-15T06:28:23.263Z"  
 }  
 },  
 {  
 "timestamp": "2016-05-15T06:28:23.263Z",  
 },   
 ...  
 \]  
}  
```

- The project aims to improve on the visualisations by using data from the search API.  
- Analysis of tweets will be done using javascript libraries like D3.js, to produce interactive visualisations.
- The “timestamp”, “retweet\_count”, “hashtags” and “favourites\_count” data fields will be used to update the app without needing to refresh the page by polling the loklak server and front-end updates in real time. For example we can show:  
	- Top hashtag count & users(by retweet/likes) for each day/ hour/ since the 1st day, using bubble charts:  
stacked bar charts to show popularity of selected hashtags, divided into 2 groups for each day radar charts for selected hashtag over time of day  
	- The “mentions” data field will be used for a directed social graph.

### Benefits for loklak
*   More compelling visualizations for loklak which let users make use of loklak_server for their events.
*   An app base for loklak users to easily use data from loklak servers, giving users alternatives to loklak.net when loklak.org is not available, and the simple front end apps on loklak.org, which do not currently update in real time, like loklak.net.
*   An app base in Javascript which encourage developers to contribute to loklak front ends, or modify it for their own needs

### Project Plan
| Time  (weeks)      | Task           |
| ------------- |:-------------|
| 2 | Develop the visualizations, using the existing data from loklak server, and libraries like Highcharts or D3 to display the data.
| 1 | Develop from ‘outside in’ : Make the shell of the app, ie. the layout and structure
| 1 | Develop the home page & server data processing to better diff the tweets and reduce the current memory load, which was a challenge for the prototype, and implement lazy loading when the tweets overflow.
| 1 | Develop the tweet grid wall |
| 1 | Develop the data visualisation combi 1 |
| 1 | Develop the data visualisation combi 2 |
| 1 | Develop settings page and function for user to add settings |
| 1 | Develop settings page to allow user to add panels |
| 2  | Tie up loose ends and make screencasts and documentation |