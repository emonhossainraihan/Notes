## heading links
- [navigation bar with active functionality](#navigation-bar-with-active-functionality)
- [Include Bootstrap in index-file](#include-bootstrap-in-index-file)
- [Error handling in express](#error-handling-in-express)
- [mongoose remove relational document](#mongoose-remove-relational-document)
- [Handle token expire](#handle-token-expire)
- [How to Integrate Disqus](#how-to-integrate-disqus)
- [HandleChange shortcut](#handleChange-shortcut)

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
