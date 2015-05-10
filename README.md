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


