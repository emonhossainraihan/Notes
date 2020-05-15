## Handling Errors
Express.js makes it a breeze to handle errors in your routes.

Express lets you centralizes your error-handling through middleware.

Let's look at patterns for how to get the most out of your error-handling.

First, our error-handling middleware looks like this:

```js
app.use(function(err, req, res, next) {
  console.error(err.message); // Log error message in our server's console
  if (!err.statusCode) err.statusCode = 500; // If err has no specified error code, set error code to 'Internal Server Error (500)'
  res.status(err.statusCode).send(err.message); // All HTTP requests must have a response, so let's send back an error with its status code and message
});
```

Express.js interprets any route callback with four parameters as error-handling middleware. Our first parameter is `err`. You should put error-handling middleware at the end of your routes and middleware in your application. This let's you catch any thrown errors.

Upstream, we can follow a simple pattern to help us understand and identify errors. Let's look at some examples.

```js
...

app.get('/forbidden', function(req, res, next) {
  let err = new Error(`${req.ip} tried to access /Forbidden`); // Sets error message, includes the requester's ip address!
  err.statusCode = 403;
  next(err);
});

...

```

Here's how we can handle a forbidden request. To use our error-handling middleware we need two things.

First, we need to throw or create an Error. When we create our error, we can define a `.message` property by passing in a string as a parameter. This becomes our `err.message`, used later. Now we can attach a status code appropriate to the problem. We should give Forbidden requests the `403` status code.

The last and most important step is to pass a parameter into `next()`. If next receives an argument, Express will assume there was an error, skip all other routes, and send whatever was passed to any error-handling middleware you have defined.

Our `next(err)` is like saying 'This is definitely an error. Go straight to error handling!'

Our middleware can now use `.message` and `.statusCode` to log, handle, and communicate the specifics of our error.

### Generalized Error Handling
Obviously, we can't specify routes like `/forbidden` for every possible broken or wrong route in our application.

How can we handle a broader set of errors?

Using the wildcard '\*' in our route, we can create a route to catch every request to a route we *have not defined elsewhere*.

```js
...

app.get('*', function(req, res, next) {
  let err = new Error('Page Not Found');
  err.statusCode = 404;
  next(err);
});

...
```

This route will grab anything that isn't routed elsewhere and throw a generic '404' error.

But we can do even better.

Let's refactor our route to make it tell Express that users shouldn't just receive an error, but should be redirected to an error page.

```js
...

app.get('*', function(req, res, next) {
  let err = new Error(`${req.ip} tried to reach ${req.originalUrl}`); // Tells us which IP tried to reach a particular URL
  err.statusCode = 404;
  err.shouldRedirect = true; //New property on err so that our middleware will redirect
  next(err);
});

...
```

Let's spice things up a bit in our middleware...

```js
...

app.use(function(err, req, res, next) {
  console.error(err.message);
  if (!err.statusCode) err.statusCode = 500; // Sets a generic server error status code if none is part of the err

  if (err.shouldRedirect) {
    res.render('myErrorPage') // Renders a myErrorPage.html for the user
  } else {
    res.status(err.statusCode).send(err.message); // If shouldRedirect is not defined in our error, sends our original err data
  }
});

...
```

Now our application will send any requests to nonsensical routes directly to an error page. On our server, we'll still see error data.

That's it. Remember:
1. throw your `Error` with a useful message
2. pass a parameter into `next()`
3. centralize your errors in error-handling middleware!

Good luck!
