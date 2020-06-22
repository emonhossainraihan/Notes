## navigation bar with active functionality:

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
## Include Bootstrap in index.html:
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
