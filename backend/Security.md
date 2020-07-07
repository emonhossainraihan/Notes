## Security considerations

If you’re working with forms and sessions on the Internet, you need to be aware of common security holes in web applications. The best security advice I’ve been given is "Never trust the client!"

## TLS over HTTPS

https://www.sitepoint.com/how-to-use-ssltls-with-node-js/

## Wear Your Helmet

There’s a neat little middleware called helmet that adds some security from HTTP headers. It’s best to include right at the top of your middlewares

## Cross-site Request Forgery (CSRF)

You can protect yourself against cross-site request forgery by generating a unique token when the user is presented with a form and then validating that token before the POST data is processed. There’s a middleware to help you out here as well:

```js
// routes.js
const csrf = require('csurf');
const csrfProtection = csrf({ cookie: true });
```

In the GET request, we generate a token:
```js
// routes.js
router.get('/contact', csrfProtection, (req, res) => {
  res.render('contact', {
    data: {},
    errors: {},
    csrfToken: req.csrfToken()
  });
});
```

And also in the validation errors response:
```js
router.post('/contact', csrfProtection, [
  // validations ...
], (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.render('contact', {
      data: req.body,
      errors: errors.mapped(),
      csrfToken: req.csrfToken()
    });
  }

  // ...
});
```

Then we just need include the token in a hidden input:
```html
<!-- view/contact.ejs -->
<form method="post" action="/contact" novalidate>
  <input type="hidden" name="_csrf" value="<%= csrfToken %>">
  <!-- ... -->
</form>
```

That’s all that’s required.

We don’t need to modify our POST request handler, as all POST requests will now require a valid token by the csurf middleware. If a valid CSRF token isn’t provided, a ForbiddenError error will be thrown, which can be handled by the error handler defined at the end of server.js.

## Cross-site Scripting (XSS)

You need to take care when displaying user-submitted data in an HTML view as it can open you up to cross-site scripting(XSS). All template languages provide different methods for outputting values. The EJS <%= value %> outputs the HTML escaped value to protect you from XSS, whereas <%- value %> outputs a raw string.

Always use the escaped output <%= value %> when dealing with user-submitted values. Only use raw outputs when you’re sure that it’s safe to do so.

## File Uploads

Uploading files in HTML forms is a special case that requires an encoding type of "multipart/form-data". See [MDN’s guide to sending form data](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data#A_special_case_sending_files) for more details about what happens with multipart form submissions. [Here](https://stackabuse.com/handling-file-uploads-in-node-js-with-expres-and-multer/) a more in-depth tutorial.
