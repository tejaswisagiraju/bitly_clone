# Intro Express
## Bitly Clone

### Getting Started

* Fork this repo
* Clone it
* Do you work
* `git add . -A && git commit -m "solution"`
* Then `git push origin master`

## Helpful Concepts

 ![the request response cycle](req_res.gif)

### Bitly Clone Spec

* A **user** can go to `localhost:3000` or the root route and see a `form` to submit a `url`.
* A **user** can submit a link to server using the form.
* A **user** should receive back a response with the url they can use to be redirected to the their submited url, i.e. something of the form  `localhost:3000/urls/:id`.
* A **user** should be able to to use the url they receive back from your server as a redirect url.

![user story](user_story.gif)

## Notes

### Deliverables

You'll need the following routes:

* `GET /` to view a form
* `POST /urls` to submit a url to shorten
* `GET /urls/:id` to be redirected to a url

* When your **Express** server receives a `url` from the client you should store push it into an array.
* When your **Express** server responds it should use the `indexOf` the url in the `urls` array as the `id`, e.g. if I am submit the first url I should get back the following:
	
	```
	View your url at localhost:3000/urls/0
	
	```
* When your user goes to the `/urls/:id` route you should look up the `:id` param in the `urls` array to find the associated url and then redirect to it.

## More Directions

### Getting Started

* Make the required files and directories

	```
	touch index.js
	mkdir views
	touch views/home.html
	npm init 
	npm install --save express
	```
* We'll need the `views/home.html` file to send as our response later.
* The `index.js` file holds all of our main application logic
* Make sure to setup your application to require **Express**.
	
	```
	var express = require("express");
	var app = express();
	```
* Then add your first simple route.

	```
	app.get("/", function (req, res) {
		res.send("Hello World");
	});
	```
* Then make sure your application server is listening.

	```
	app.listen(3000, function (req, res) {
		console.log("working!!")
	});
	```
* You should have an `index.js` like the following:

	```
	var express = require("express");
	var app = express();

	app.get("/", function (req, res) {
		res.send("Hello World");
	});

	app.listen(3000, function (req, res) {
		console.log("working!!")
	});
	```

* Create a `GET /` route in your 

### HINT DIRECTIONS

* We want to send our `./views/home.html` file as our response when someone goes to **localhost:3000/**.

	```
		./views/home.html
   /|\
		|
	the dot means 
	current 
	working 
	directory
	or cwd
	```
* We want to add some text to the `home.html` file

	```html
	<!DOCTYPE html>
<html>
<head>
  <title>Bitly Clone</title>
</head>
<body>
	Hello World
</body>
</html>
	```
* Let's send that file back as a response. Modify your `app.get("/", ...)` route.

	```javascript
	app.get("/", function (req, res) {
		// note we are using `res.sendFile`
		res.sendFile(process.cwd() + "/views/home.html");
	});
	```
* Test your app is running at `localhost:3000`. Do you see hello world?


#### Quick Refactor

Just concatenating strings that represent file paths is dangerous. Just imagine adding up.

```
"/views/" + "/home.html"
```

Because it would be tough for you to go in and properly remove all the extra `/` marks or add them when missing we will a built in Node utility to do this for us **path**. Add the following to the top of your application.

```
var express = require("express");
var path = require("path"); // <-- add this

var app = express();

```

Then define your a variable that is your `./views/` directory path.

```
var express = require("express");
var path = require("path"); 

var app = express();
// a variable that represents "./views"
var views = path.join(process.cwd(), "views"); 

```

Now in your `app.get` you should refactor your to use this.

```
app.get("/", function (req, res) {
	var homePath = path.join(views, "home.html");
	res.sendFile(homePath);
});

```

-----

### Adding A Form

We want to add a form for our `urls`. 

```html

<form action="/urls" method="POST">
	<input type="text" name="url" placeholder="New Url">
	<button>Shorten URL</button>
</form>

```


### Pitfalls

* Make sure you check that each submited url starts with either `https://` or `http://`.

### Bonus

Because using an array index doesn't obscure how we are looking up the submitted links our site is vulnerable. Nothing is stopping someone from going through every index to see what links have been submitted.

```
localhost:3000/urls/0
   localhost:3000/urls/1
   localhost:3000/urls/2
   ...
   localhost:3000/urls/99
```

* Use `npm` to install `secure-random`
	* `npm install --save randomstring`
	*  In the node repl verify the following behavior.
	
		```
		var RString = require("randomstring");
		RString.generate()
		// => "XwPp9xazJ0ku5CZnlmgAx2Dld8SHkAeT"
		
		```
	* Get rid of your array and use an Object to store a random string associated to random string generated each time a url is submitted.
	* Your server should now respond with the following:
	
		```
		Visit your url at localhost:3000/urls/XwPp9xazJ0ku5CZnlmgAx2Dld8SHkAeT
		```


