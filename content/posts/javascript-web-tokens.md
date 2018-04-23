---
title: "Javascript web tokens"
date: 2016-07-22T18:08:59-07:00
draft: true
---

In the loklak webclient, the plan is to store the accounts locally on mongodb, instead of using loklak server to store accounts. Here’s how I used [JWTs](http://jwt.io) (JSON Web Tokens) for basic user authentication.

Token based authentication is often compared to the traditional method of saving a session id in a cookie, and checking the user information in the session object for every request. Instead of saving user information on the server and checking it every request, a token, which allows access to the server, is given in exchange for valid credentials and is sent in every request. There are various advantages to this, including [security and scaling](https://jwt.io/introduction/).

As you can see from the mongoose user schema the hash and salt are stored instead of a password.

```js
var UserSchema = new Schema({  
	email: {  
		type: String,  
		unique: true,  
		required: true  
	},  
	name: {  
		type: String,  
		required: true  
	},  
	hash: String,  
	salt: String  
});  	
```
[PassportJS](http://passportjs.org/) is a popular middleware for handling authentication in Node.js, and since the web client already uses Express, this was a natural choice. Various authentication ‘strategies’ will also be used to connect other social login providers, like twitter, or weibo.

For now, since we are using email to store the username and hashed password, we will choose the [passport-local](https://www.npmjs.com/package/passport-local) package. The configuration is easy can be found on their [website](http://passportjs.org/docs/configure), and is almost the same, except that we use email as username, rather than just any string.

```js
passport.use(new LocalStrategy({  
	usernameField: 'email'  
	},  
	function(username, password, done) {  
		// ... same as example config in passportjs.org  
	}  
}) 	 
```
Express.js then handles the register routes:
  
```js
router.post('/register', function(req,res){  
	passport.authenticate('local', function(err, user, info){  
	var token;  
	// If Passport throws/catches an error  
	if (err) {  
		res.status(404).json(err);  
		return;  
	}  
	// If a user is found, log in, ie. return a token  
	if(user){  
		token = user.generateJwt();  
		res.status(200);  
		res.json({  
			token : token  
		});  
	} else {  
		// If user is not found, register the user, then return a token  
		var user = new User();  
		user.name = req.body.email;  
		user.email = req.body.email;  
		user.setPassword(req.body.password);  
		user.save(function(err) {  
			var token;  
			token = user.generateJwt();  
			res.status(200);  
			res.json({  
				token : token  
			});  
		});  
	}  
});
```
So that’s it for the server side code, and similar logic can be applied to the login route. Now we move on to AngularJS, where we use a service to access the above route. As you can see below the JWT is stored in local storage when the user logs in.

```js
function AuthenticationService($http, $window) {  
  
	var saveToken = function (token) {  
		$window.localStorage\['jwt-token'\] = token;  
	};  

	var getToken = function () {  
		return $window.localStorage\['jwt-token'\];  
	};  

	var isLoggedIn = function() {  
		var token = getToken();  
		var payload;  

		if(token){  
			payload = token.split('.')\[1\];  
			payload = $window.atob(payload); // .atob decodes the base64 String  
			payload = JSON.parse(payload);  
			return payload.exp < Date.now() / 1000;  
		} else {  
			return false;  
		}  
	};  

	var currentUser = function() {  
		if(isLoggedIn()){  
			var token = getToken();  
			var payload = token.split('.')\[1\];  
			payload = $window.atob(payload);   
			payload = JSON.parse(payload);  
			return {  
				email : payload.email,  
				name : payload.name  
			};  
		}  
	};  

	var register = function(user) {  
		return $http.post('/api/register', user).success(function(data){  
			saveToken(data.token);  
		});  
	};  	
}
```
The last step is to use these services, which in turn uses our routes, in our controllers:


```js
$rootScope.root.onSubmit = function () {  
	AuthService  
	.register($rootScope.root.credentials)  
	.error(function(err){  
		alert(err);  
	})  
	.then(function(){  
		$location.path('/');  
		$rootScope.root.isLoggedIn = AuthService.isLoggedIn();  
		$rootScope.root.currentUser = AuthService.currentUser();  
	});  
};  

$rootScope.root.onLogout = function () {  
	AuthService.logout();  
	$location.path('/');  
	$rootScope.root.isLoggedIn = AuthService.isLoggedIn();  
};  

```

There is still more work to be done to connect this with the rest of the app, but hopefully those reading can better understand how to implement local auth using JWTs on their SPA.