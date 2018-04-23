---
title: "CS3216 Progressive Web Apps"
date: 2016-08-23T18:28:33-07:00
draft: true
---

# PWA
The second project is on Progressive Web Apps. Our project title is called Drop In.

## Drop in 
So my group decided on doing an app using emoji’s and mapbox, it’s Pokemon Go for chat where you (or businesses) could drop and discover messages (mockup screenshots below) around you. Well apparently you need to [upload images to Mapbox Studio for custom marker icons in MapboxGL](https://www.mapbox.com/help/custom-markers/), the only other custom image (using css) was the user’s avartar which updated on map move rather than as a marker property. We thought we could upload all 1200+ emoji / 874 twemoji svg files and be done (they were only 1mb, why not?), but strangely there was an (AFAIK) undocumented 500 sprite limit.

### Lesser is better
Anyway emojis like flags and letters would not be too relevant for our purposes, so we just had to choose which emojis we would support. Based on [http://emojitracker.com/](http://emojitracker.com/) we thought that using the people + objects categories (425 twemojis) would cover well enough ground for people to post their reactions and activities.

### How to get the files
The next step was to upload the svgs. To choose a subset of [twemojis](https://github.com/twitter/twemoji) to upload to Mapbox Studio and to use with [react-emoji-picker](https://github.com/chadoh/react-emoji-picker) in our app, I had to first get the list of twemojis we would support on the emoji picker. There was a category to name mapping, then there was a name to unicode mapping, put the two together and a script and tada I got the files, here’s [the repo](https://github.com/leonmak/twemoji-filter).


## Quick wins - get offline with sw-precache
Progressive web apps have offline functionality because of service workers caching static assets and data.  
Starting with the create-react-app boiler plate, you can easily see this in action by following the steps in [https://github.com/jeffposnick/create-react-pwa](https://github.com/jeffposnick/create-react-pwa/compare/starting-point...pwa).

As with any PWA, first make a [manifest.json](https://developers.google.com/web/updates/2014/11/Support-for-installable-web-apps-with-webapp-manifest-in-chrome-38-for-Android?hl=en) in the root folder and link it in `index.html`, and also register the service worker that will be created by sw-precache:  

```html
<link rel="manifest" href="manifest.json">  
<!-- ... -->  
<!-- At the bottom -->  
<script>  
if ('serviceWorker' in navigator) {  
 navigator.serviceWorker.register('service-worker.js').catch(function(ex) {  
 console.warn(ex);  
 });  
}  
</script>  
```
Then in the package.json under “scripts”:  
```js
"scripts": {  
 "build": "react-scripts build && cp manifest.json build/ && sw-precache --root='build/' --static-file-globs='build/**/!(\*map\*)'",  
}  
```
This creates the production build, copies the manifest.json into the build folder, runs sw-precache in the build folder and caches the static content.

To deploy the static build to a https I used surge.sh, assuming you’re in the root folder, I can deploy to a https domain to see it working:  

```sh
surge client/build --domain drop-dev.surge.sh  
```

Open chrome dev tools under application you should see it:  
![ss](http://res.cloudinary.com/leonmak/image/upload/v1474339302/ss-blog-post-6_bqobvk.png)

## Deploying
As assignment 3 comes to an end, just wanna share a few options for deploying the app which was built upon the [create-react-app](https://github.com/facebookincubator/create-react-app) boilerplate.

### The no-backend option
These are mainly the options stated in the [documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#deployment), which makes use of the build folder generated with npm build script command. There’s heroku buildpacks, modulus, now and surge.sh, and probably more being added soon. These had been useful for deploying the frontend of the app, when our backend was not ready yet.

### With backend
Deploying the backend comes with its own set of challenges. Before that, developing one to work with the webpack dev server was also a tough one, here’s a [tutorial that came in useful for us](https://www.fullstackreact.com/articles/using-create-react-app-with-a-server/). We eventually used Express for our backend, and it worked out quite well for us.

These were steps that worked from the [discussion in the issues](https://github.com/facebookincubator/create-react-app/issues/639):

*   Delete `Procfile`
*   Move `"react-scripts": "0.2.3"` from devDependencies to dependencies

In the server’s package.json:
To tell npm to run server.js:
*   Replace `"start": "nf start -p 3000"` with `"start": "node server.js"`

Heroku runs [npm install](https://devcenter.heroku.com/articles/nodejs-support#build-behavior) as default so to also run `npm i` in our client build’s `package.json`:
*   Replace `"server": "API_PORT=3001 ./node_modules/.bin/babel-node server.js` with `"install": "cd client && npm install && npm run build && cd .."`

### In server.js

To tell serve up the static site on the root domain:

*   Add `const path = require('path');`
*   Replace `process.env.API_PORT` with `process.env.PORT`
*   Add `app.use(express.static(path.join(__dirname, 'client/build')));`

In `routes.js` (or wherever your routes are)
```js
var router = express.Router();  
  
router.get('/api/feeds', FeedsController.getFeeds); // Get all the feeds  
  
// ... other API routes  
  
router.get('*', function (req, res) {  
 res.sendFile(path.join(__dirname, '/../client/build/index.html'))  
})  
```

This is so that if I type in a direct url for example [https://dropins.space/drops](https://dropins.space/drops), I won’t get a 404.

It sucks that heroku puts dynos to sleep, but a _simple_ workaround I googled was to do:  

```js
var https = require("https");  
setInterval(function() {  
 https.get("https://dropins.space");  
}, 300000); // ping every 5 minutes (300000)  
```
