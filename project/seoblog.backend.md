course-git-repo : https://github.com/kaloraat/react-node-next-multi-user-blogging-platform/

## Initial files + command:

- npm init -y
- touch .gitignore
- touch .env

`npm i express mongoose body-parser cookie-parser morgan nodemon dotenv cors`

## dependencies info:

- **body-parser** : in order to read HTTP POST data , we have to use "body-parser" node module. body-parser is a piece of express middleware that reads a form's input and stores it as a javascript object accessible through req.body
- **cors** : Cross-origin resource sharing (sharing things between frontend to backend in different port. It's for only browser to browser communication)
- <del>**app.get('endpoint', callback_function)**</del>
- <del>**dotenv** - accessing variable(process.env) in .env file</del>
- <del>`pkill -8000 node`</del>
- <del>**{timestamps: true}** : This option adds createdAt and updatedAt properties that are timestamped with a Date</del>
- **express-validator** : `npm install express-validator@5.3.0` [demo](#express-validator)
- **jsonwebtoken** :
  1. jwt.sign(payload, secretOrPrivateKey, [options, callback])
  2. jwt.verify(token, secretOrPublicKey, [options, callback])
  3. jwt.decode(token [, options])
- **express-jwt** : Middleware that validates JsonWebTokens and check expiry or not.
- **formidable** : A Node.js module for parsing form data, especially file uploads.
- **string-strip-html** : Strips HTML tags from strings. Detects legit unencoded brackets.
- **slugify** : slugify generate slug which help to SEO(not using id) and meta description.
- **lodash** : Lodash makes JavaScript easier by taking the hassle out of working with arrays, numbers, objects, strings, etc.
- **shortid** : default username

## some list of [books](https://flaviocopes.com/page/list-subscribed/) from flaviocopes

## Authentication

---

<center> <h1>SignUp</h1> </center>

## validator

`const {check} = require('express-validator')`
we can exports a list of checking with `.not()`, `.isEmpty()` , `.withMessage(msg)`, `isEmail()` and `isLength({min: #})` => validator

```js
const check = require("express-validator/check").check;
exports.userSignupValidator = [
  //checking every field
];
exports.userSigninValidator = [
  //checking every field
];
```

**validator=>Index.js**
`const {validationResult} = require('express-validator')`
After validator we pass the req to validationResult to catch the errors.

```js
const validationResult = require("express-validator/check").validationResult;
exports.runValidation = (req, res, next) => {
  const errors = validationResult(req);
  //handle if error found
  next();
};
```

**router.post("endpoint", userSignupValidator, runValidation, callback);**

## Hashing-Password

### In models:

If you have Buffer data then you need to define `contentType: String` also.

```js
userSchema
  .virtual("password")
  .set(function(password) {
    // create a temporarity variable called _password
    this._password = password;
    // generate salt
    this.salt = this.makeSalt();
    // encryptPassword
    this.hashed_password = this.encryptPassword(password);
  })
  .get(function() {
    return this._password;
  });

userSchema.methods = {
    authenticate: function(plainText) {
    return this.encryptPassword(plainText) === this.hashed_password;
  },
    encryptPassword: ...,
    makeSalt: ...
}
```

crypto used in encryptPassword `.createHmac('sha1', this.salt) update(password) .digest('hex')` [stackoverflow-Link](https://stackoverflow.com/a/18813806/9138425)

- **crypto.createHmac(algorithm, key)** : Creates and returns a hmac object, a cryptographic hmac with the given algorithm and key.
- **hmac.update(data)**
- **hmac.digest([encoding])**

## Controller:

```js
const User = require("../models/user");
const shortId = require("shortid");
exports.signup = (req, res) => {
  const { name, email, password } = req.body;
  //find is the user exist in db
  //User.findOne({ email: email }).exec(callback)
  // if not then create a new user and save
  //newUser = new User({data})
  //newUser.save(callback)
```

---

<center> <h1>SignIn</h1> </center>

## controller-signin:

```js
const jwt = require("jsonwebtoken");
const expressJwt = require("express-jwt");
exports.signin = (req, res) => {
  const { email, password } = req.body;
  // check if user exist
  // authenticate
  // generate a token and send to client
  res.cookie("token", token, { expiresIn: "1d" });
  const { _id, username, name, email, role } = user;
  return res.json({
    token,
    user: { _id, username, name, email, role }
  });
};
```

<center> <h1>Protected Route</h1> </center>
## controller:

```js
const expressJwt = require("express-jwt");
exports.requireSignin = expressJwt({
  secret: process.env.JWT_SECRET
});
// requireSignin have JWT_SECRET which can extract other information of the token (when we signin)
// like _id and expire in which we user to generate the token

exports.authMiddleware = (req, res, next) => {
  const authUserId = req.user._id;
  User.findById({ _id: authUserId }).exec((err, user) => {
    if (err || !user) {
      return res.status(400).json({
        error: "User not found"
      });
    }
    req.profile = user;
    next();
  });
};
exports.adminMiddleware = (req, res, next) => {...}
```

then we can you this controller as middleware `router.get("endpoint", requireSignin, callback)`

---

| HTTP-code | Description                                                                                                                                |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 200       | Successful request                                                                                                                         |
| 204       | Successful request; no response body                                                                                                       |
| 400       | Bad request                                                                                                                                |
| 401       | Authorization error                                                                                                                        |
| 403       | Forbidden: access is not permitted                                                                                                         |
| 416       | Requested range not satisfiable: server cannot retrieve the portions of the file that are specified in the HTTP range header               |
| 500       | Internal server error                                                                                                                      |
| 404       | broken or dead link                                                                                                                        |
| 304       | Not Modified is an HTTP status code that is returned to the client when the cached copy of a particular file is up to date with the server |

## Extra information:

**<center>During creating this project</center>**

- If your callback function return several responses depending on the logic of the application and if you don't use explicit return

```js
router.post("/url", function(req, res) {
// (logic1)
     res.send(200, { response: 'response 1' });
   // (logic2)
      res.send(200, { message: 'response 2' });
    }})
```

then it will throw this error: `Error [ERR_HTTP_HEADERS_SENT]: Cannot set headers after they are sent to the client`.Which can actually be solved with using return. It can also be solved with using if else clauses.

- **app.use([path],callback,[callback]):** If the **routes[path]** are blank, meaning that when I added those middleware layers I specified that they be triggered on any route. If I added a custom middleware layer that only triggered on the path /api/signin that would be reflected as a string in the route field of that middleware layer object in the stack printout above.

  > _Each layer is essentially adding a function that specifically handles something to your flow through the middleware._

  Each app.use(middleware) is called every time a request is sent to the server.

- **exports vs export:**

```js
// imports
// ex. importing a single named export
import { MyComponent } from "./MyComponent"; // ex. importing multiple named exports
import { MyComponent, MyComponent2 } from "./MyComponent"; // ex. giving a named import a different name by using "as":
import { MyComponent2 as MyNewComponent } from "./MyComponent"; // exports from ./MyComponent.js file
export const MyComponent = () => {};
export const MyComponent2 = () => {};
```

- **Bootstrap 4:** With [sass](https://coursetro.com/posts/code/130/Learn-Bootstrap-4-Final-in-2018-with-our-Free-Crash-Course)
- **Asynchoronus:** [here](https://plnkr.co/edit/ejxlWstPsQ8V1mw7?open=lib%2Fscript.js)
- **Manage Cookies:**
  1. `Response.cookie(name,value[,expires/domain/path...])` is used to set/get cookies and `Response.clearCookie()` is used to clear it
  2. `cookie=require('js-cookie')` has **cookie.set()**,**cookie.get()** and **cookie.remove()** methods


