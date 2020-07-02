## heading links
- [navigation bar with active functionality](#navigation-bar-with-active-functionality)
- [Include Bootstrap in index-file](#include-bootstrap-in-index-file)
- [Error handling in express](#error-handling-in-express)
- [mongoose remove relational document](#mongoose-remove-relational-document)
- [Handle token expire](#handle-token-expire)
- [How to Integrate Disqus](#how-to-integrate-disqus)
- [HandleChange shortcut](#handlechange-shortcut)
- [Forget password and Reset password](#forget-password-and-reset-password)
- [Google signup](google-signup)

## navigation bar with active functionality

```js
//app.js
import React from 'react';
import { BrowserRouter } from 'react-router-dom';
import MainRouter from './MainRouter';
import './App.css';

const App = () => (
  <BrowserRouter>
    <MainRouter />
  </BrowserRouter>
);
export default App;
//mainRouter.js
import React from 'react';
import { Route, Switch } from 'react-router-dom';
import Footer from './components/footer';
// route
import Home from './core/Home';
import Menu from './core/Menu';

const MainRouter = () => (
  <div>
    <Menu />
    <Switch>
      <Route path="/" component={Home} />
    </Switch>
    <Footer />
  </div>
);
export default MainRouter;
//menu.js
import React from 'react';
import { Link, withRouter } from 'react-router-dom';

const isActive = (history, path) => {
  if (history.location.pathname === path) return { color: '#ff9900' };
  else return { color: '#ffffff' };
};
import React from 'react';
import { Link, withRouter } from 'react-router-dom';

const isActive = (history, path) => {
  if (history.location.pathname === path) return { color: '#ff9900' };
  else return { color: '#ffffff' };
};

const Menu = ({ history }) => (
  <div>
    <ul className="nav nav-tabs bg-primary">
      <li className="nav-item">
        <Link className="nav-link" style={isActive(history, '/')} to="/">
          Home
        </Link>
      </li>
    </ul>
  </div>
);

export default withRouter(Menu);
```
## Include Bootstrap in index file
css go to head section and scripts go to at the last of the body section 
```bash
npm install bootstrap --save
npm install jquery popper.js --save
npm install font-awesome --save
npm install bootstrap-social --save
```

```html
    <!-- Required meta tags always come first -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta http-equiv="x-ua-compatible" content="ie=edge">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="node_modules/font-awesome/css/font-awesome.min.css">
    <link rel="stylesheet" href="node_modules/bootstrap-social/bootstrap-social.css">

    <!-- jQuery first, then Popper.js, then Bootstrap JS. -->
    <script src="node_modules/jquery/dist/jquery.slim.min.js"></script>
    <script src="node_modules/popper.js/dist/umd/popper.min.js"></script>
    <script src="node_modules/bootstrap/dist/js/bootstrap.min.js"></script>
```
## Error handling in express

```js
class ErrorHandler extends Error {
  constructor(statusCode, message) {
    super();
    this.statusCode = statusCode;
    this.message = message;
  }
}
const handleError = (err, res) => {
  const { statusCode, message } = err;
  res.status(statusCode).json({
    status: "error",
    statusCode,
    message
  });
};
module.exports = {
  ErrorHandler,
  handleError
};

const express = require('express')
const { handleError, ErrorHandler } = require('./helpers/error')
const app = express()

app.use(express.json())
const PORT = process.env.PORT || 3000

app.get('/', (req, res) => {
  return res.status(200).json('Hello world');
})

app.get('/error', (req, res) => {
  throw new ErrorHandler(500, 'Internal server error');
})

app.use((err, req, res, next) => {
  handleError(err, res);
});
app.listen(PORT, () => console.log(`server listening at port ${PORT}`))
```

## mongoose remove relational document

```js
clientSchema.pre('remove', function(next) {
    // 'this' is the client being removed. Provide callbacks here if you want
    // to be notified of the calls' result.
    Sweepstakes.remove({client_id: this._id}).exec();
    Submission.remove({client_id: this._id}).exec();
    next();
});
```

## Handle token expire
```js
export const handleResponse = (response) => {
  if (response.status === 401) {
    signout(() => {
      //* reDirect to signin page
      Router.push({
        pathname: '/signin',
        query: {
          message: 'Your session is expired. Please signin',
        },
      });
    });
  } else {
    return;
  }
};

export const signout = (next) => {
  removeCookie('token');
  removeLocalStorage('user');
  next();

  return fetch(`${API}/signout`, {
    method: 'GET',
  })
    .then((response) => {
      console.log('signout success');
    })
    .catch((err) => console.log(err));
};
```

## How to Integrate Disqus
**[DisqusThread.js](https://github.com/kriasoft/react-starter-kit/blob/master/docs/recipes/how-to-integrate-disqus.md)**
```js
import React from 'react';
import PropTypes from 'prop-types';

const SHORTNAME = 'example';
const WEBSITE_URL = 'http://www.example.com';

function renderDisqus() {
  if (window.DISQUS === undefined) {
    var script = document.createElement('script');
    script.async = true;
    script.src = 'https://' + SHORTNAME + '.disqus.com/embed.js';
    document.getElementsByTagName('head')[0].appendChild(script);
  } else {
    window.DISQUS.reset({ reload: true });
  }
}

class DisqusThread extends React.Component {
  static propTypes = {
    id: PropTypes.string.isRequired,
    title: PropTypes.string.isRequired,
    path: PropTypes.string.isRequired,
  };

  shouldComponentUpdate(nextProps) {
    return (
      this.props.id !== nextProps.id ||
      this.props.title !== nextProps.title ||
      this.props.path !== nextProps.path
    );
  }

  componentDidMount() {
    renderDisqus();
  }

  componentDidUpdate() {
    renderDisqus();
  }

  render() {
    let { id, title, path, ...other } = this.props;

    if (process.env.BROWSER) {
      window.disqus_shortname = SHORTNAME;
      window.disqus_identifier = id;
      window.disqus_title = title;
      window.disqus_url = WEBSITE_URL + path;
    }

    return <div {...other} id="disqus_thread" />;
  }
}

export default DisqusThread;
```
`MyComponent.js`
```js
import DisqusThread from './DisqusThread.js';

export default function MyComponent() {
  return (
    <div>
      <DisqusThread
        id="e94d73ff-fd92-467d-b643-c86889f4b8be"
        title="How to integrate Disqus into ReactJS App"
        path="/blog/123-disquss-integration"
      />
    </div>
  );
}
```
## HandleChange shortcut 
```js
 const handleChange = (name) => (e) => {
    // console.log(e.target.value);
    const value = name === "photo" ? e.target.files[0] : e.target.value;
    formData.set(name, value);
    setValues({ ...values, [name]: value, formData, error: "" });
  };
```
## Forget password and Reset password

Route: router.put('/forgot-password',forgotPasswordValidator,runValidation,forgotPassword);
```js
const nodemailer = require('nodemailer');
// mailing API authentication
const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'mdroy7475@gmail.com',
    pass: process.env.EMAIL_FROM_PASS,
  },
});

exports.forgotPassword = (req, res) => {
  const { email } = req.body;

  User.findOne({ email }, (err, user) => {
    if (err || !user) {
      return res.status(401).json({
        error: 'User with that email does not exist',
      });
    }
    // generate token 
    const token = jwt.sign({ _id: user._id }, process.env.JWT_RESET_PASSWORD, {
      expiresIn: '10m',
    });

    // email setup
    const mailOptions = {
      from: process.env.EMAIL_FROM,
      to: email,
      subject: `Password reset link`,
      html:`...`
    };
    // populating the db > user > resetPasswordLink
    return user.updateOne({ resetPasswordLink: token }, (err, success) => {
      if (err) {
        return res.json({ error: errorHandler(err) });
      } else {
        transporter.sendMail(mailOptions, (error, response) => {
          if (error) {
            console.log(error);
            return res.json({
              success: false,
            });
          }

          return res.json({
            message: `Email has been sent to ${email}`,
          });
        });
      }
    });
  });
};

//Route: router.put('/reset-password',resetPasswordValidator,runValidation,resetPassword);
// manually give the token in the resetPasswordLink field

exports.resetPassword = (req, res) => {
  const { resetPasswordLink, newPassword } = req.body;

  if (resetPasswordLink) {
    jwt.verify(resetPasswordLink, process.env.JWT_RESET_PASSWORD, function (
      err,
      decoded
    ) {
      if (err) {
        return res.status(401).json({
          error: 'Expired link. Try again',
        });
      }
      User.findOne({ resetPasswordLink }, (err, user) => {
        if (err || !user) {
          return res.status(401).json({
            error: 'Something went wrong. Try later',
          });
        }
        const updatedFields = {
          password: newPassword,
          resetPasswordLink: '',
        };

        user = _.extend(user, updatedFields);

        user.save((err, result) => {
          if (err) {
            return res.status(400).json({
              error: errorHandler(err),
            });
          }
          res.json({
            message: `Great! Now you can login with your new password`,
          });
        });
      });
    });
  }
};

```

To grap the `resetPasswordLink` from the URL we need to use `withRouter` (query.id) in our frontend
```js
//! forget && reset password frontend
export const forgotPassword = (email) => {
  return fetch(`${API}/forgot-password`, {
    method: 'PUT',
    headers: {
      Accept: 'application/json',
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(email),
  })
    .then((response) => {
      return response.json();
    })
    .catch((err) => console.log(err));
};

//* resetInfo => resetPassword
export const resetPassword = (resetInfo) => {
  return fetch(`${API}/reset-password`, {
    method: 'PUT',
    headers: {
      Accept: 'application/json',
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(resetInfo),
  })
    .then((response) => {
      return response.json();
    })
    .catch((err) => console.log(err));
};
```

## Google signup 
```js
//controller.js
const { OAuth2Client } = require('google-auth-library');
const client = new OAuth2Client(process.env.GOOGLE_CLIENT_ID);
exports.googleLogin = (req, res) => {
  const idToken = req.body.tokenId;
  client
    .verifyIdToken({ idToken, audience: process.env.GOOGLE_CLIENT_ID })
    .then((response) => {
      // console.log(response)
      const { email_verified, name, email, jti } = response.payload;
      if (email_verified) {
        User.findOne({ email }).exec((err, user) => {
          if (user) {
            // console.log(user)
            const token = jwt.sign({ _id: user._id }, process.env.JWT_SECRET, {
              expiresIn: '1d',
            });
            res.cookie('token', token, { expiresIn: '1d' });
            const { _id, email, name, role, username } = user;
            return res.json({
              token,
              user: { _id, email, name, role, username },
            });
          } else {
            let username = shortId.generate();
            let profile = `${process.env.CLIENT_URL}/profile/${username}`;
            let password = jti; //*use it for are process sync to google
            user = new User({ name, email, profile, username, password });
            user.save((err, data) => {
              if (err) {
                return res.status(400).json({
                  error: errorHandler(err),
                });
              }
              const token = jwt.sign(
                { _id: data._id },
                process.env.JWT_SECRET,
                { expiresIn: '1d' }
              );
              res.cookie('token', token, { expiresIn: '1d' });
              const { _id, email, name, role, username } = data;
              return res.json({
                token,
                user: { _id, email, name, role, username },
              });
            });
          }
        });
      } else {
        return res.status(400).json({
          error: 'Google login failed. Try again.',
        });
      }
    });
};
```
In frontend part:
```js
//! google login action
export const loginWithGoogle = (user) => {
  return fetch(`${API}/google-login`, {
    method: 'POST',
    headers: {
      Accept: 'application/json',
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(user),
  })
    .then((response) => {
      return response.json();
    })
    .catch((err) => console.log(err));
};
// component
import Link from 'next/link';
import { useState, useEffect } from 'react';
import Router from 'next/router';
import GoogleLogin from 'react-google-login';
import { loginWithGoogle, authenticate, isAuth } from '../../actions/auth';
import { GOOGLE_CLIENT_ID } from '../../config';

const LoginGoogle = () => {
  const responseGoogle = (response) => {
    //console.log(response);
    const tokenId = response.tokenId;
    const user = { tokenId };
    loginWithGoogle(user).then((data) => {
      console.log(data);
      if (data.error) {
        console.log(data.error);
      } else {
        authenticate(data, () => {
          if (isAuth() && isAuth().role === 1) {
            Router.push(`/admin`);
          } else {
            Router.push(`/user`);
          }
        });
      }
    });
  };

  return (
    <div className="pb-3">
      <GoogleLogin
        clientId={`${GOOGLE_CLIENT_ID}`}
        buttonText="Login with Google"
        onSuccess={responseGoogle}
        onFailure={responseGoogle}
        theme="dark"
      />
    </div>
  );
};

export default LoginGoogle;
```
