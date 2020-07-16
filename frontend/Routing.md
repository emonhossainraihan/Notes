# react router dom

React router gives us three components [Route,Link,BrowserRouter] which help us to implement the routing.

In the Route component, we need to pass the two props
- path: it means we need to specify the path.
- component: which component user needs to see when they will navigate to that path.

```js
 <Router>
    <div>
      <Route exact path="/" component={App} />
      <Route path="/users" component={Users} />
      <Route path="/contact" component={Contact} />
    </div>
  </Router>
```

## Adding Navigation using Link component

```js
  <Router>
    <div>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/users">Users</Link>
        </li>
        <li>
          <Link to="/contact">Contact</Link>
        </li>
      </ul>
      <Route exact path="/" component={App} />
      <Route path="/users" component={Users} />
      <Route path="/contact" component={Contact} />
    </div>
  </Router>
```

## How to add a 404 page in react?

we need to import another component called Switch which is provided by the react router. 
Switch component helps us to render the components only when path matches 
otherwise it fallbacks to the component which haven't given any path.

```js
  <Router>
    <div>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/users">Users</Link>
        </li>
        <li>
          <Link to="/contact">Contact</Link>
        </li>
      </ul>
      <Switch>
        <Route exact path="/" component={App} />
        <Route path="/users" component={Users} />
        <Route path="/contact" component={Contact} />
        <Route component={Notfound} />
      </Switch>
    </div>
  </Router>
```

## Url Parameters
Url parameters helps us to render the same component based on its dynamic url like in our Users component 
assume that they are different users with id 1,2,3. You can grap the id from params object inside Users component.

```js
<Route path="users/:id" component={Users} />
```

> You can grap props inside Users component whatever it is class or functional component.

## Nested Routes

Nested routing helps us to render the sub-routes like users/1, users/2 etc.

## How to implement a nested routing?

In the users.js file, we need to import the react **router** components because 
we need to implement the subroutes inside the Users Component.

```js
//users.js
import React from 'react'
import { Route, Link } from 'react-router-dom'

const User = ({ match }) => <p>{match.params.id}</p>

class Users extends React.Component {
  render() {
    const { url } = this.props.match
    return (
      <div>
        <h1>Users</h1>
        <strong>select a user</strong>
        <ul>
          <li>
            <Link to="/users/1">User 1 </Link>
          </li>
          <li>
            <Link to="/users/2">User 2 </Link>
          </li>
          <li>
            <Link to="/users/3">User 3 </Link>
          </li>
        </ul>
        <Route path="/users/:id" component={User} />
      </div>
    )
  }
}export default Users
```

## NavLink

It is used to style the active routes so that user knows on which page 
he or she is currently browsing on the website.

## What is the difference between NavLink and Link?

The link is used to navigate the different routes on the site. 
But NavLink is used to add the style attributes to the active routes.

```js
  <Router>
    <div>
      <ul>
        <li>
          <NavLink exact activeClassName="active" to="/">
          //here active class reflect the active route
            Home
          </NavLink>
        </li>
        <li>
          <NavLink activeClassName="active" to="/users">
            Users
          </NavLink>
        </li>
        <li>
          <NavLink activeClassName="active" to="/contact">
            Contact
          </NavLink>
        </li>
      </ul>
      <hr />
      <Switch>
        <Route exact path="/" component={App} />
        <Route path="/users" component={Users} />
        <Route path="/contact" component={Contact} />
        <Route component={Notfound} />
      </Switch>
    </div>
  </Router>
```

## What is Programmatic navigation?

It means we need to redirect the user when an event happens on that route.

To navigate programmatically we need to take the help of history object which is passed by the react-router.


