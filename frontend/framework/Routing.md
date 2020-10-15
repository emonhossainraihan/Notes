# react router dom

When you need to navigate through a React application with multiple views, you’ll need a router to manage the URLs. React Router takes care of that, keeping your application UI and the URL in sync.

- [Router](#router)
- [history](#history)
- [Adding Navigation using Link component](#adding-navigation-using-link-component)
- [How to add a 404 page in react?](#how-to-add-a-404-page-in-react)
- [Url Parameters](#url-parameters)
- [Nested Routes](#nested-routes)
- [How to implement a nested routing?](#how-to-implement-a-nested-routing)
- [NavLink](#navlink)
- [Protecting Routes](#protecting-routes)
- [Custom Route](#custom-route)

React router gives us three components [Route,Link,BrowserRouter] which help us to implement the routing.

## Router

You need a router component and several route components to set up a basic route as exemplified above. Since we’re building a browser-based application, we can use two types of routers from the React Router API:

- `<BrowserRouter>`: it uses the HTML5 History API to keep track of your router history
- `<HashRouter>`: it uses the hash portion of the URL (`window.location.hash`) to remember things

The primary difference between them is evident in the URLs that they create:

```js
// <BrowserRouter>
http://example.com/about

// <HashRouter>
http://example.com/#/about
```

>Note: A router component can only have a single child element. The child element can be an HTML element — such as div — or a react component.

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

BrowserRouter creates an instance of history for our entire App component. Let me formally introduce you to history.

## history

>history is a JavaScript library that lets you easily manage session history anywhere JavaScript runs. history provides a minimal API that lets you manage the history stack, navigate, confirm navigation, and persist state between sessions. — React Training docs

Each router component creates a history object that keeps track of the current location (`history.location`) and also the previous locations in a stack. When the current location changes, the view is re-rendered and you get a sense of navigation. How does the current location change? The history object has methods such as `history.push()` and `history.replace()` to take care of that. `history.push()` is invoked when you click on a `<Link>` component, and `history.replace()` is called when you use `Redirect>`. Other methods — such as `history.goBack()` and `history.goForward()` — are used to navigate through the history stack by going back or forward a page.

## `<Route>` and `<Link>` 

The `<Route>` component is the most important component in React router. It renders some UI if the current location matches the route’s path. Ideally, a `<Route>` component should have a prop named path, and if the pathname is matched with the current location, it gets rendered.

The `<Link>` component, on the other hand, is used to navigate between pages. It’s comparable to the HTML anchor element. However, using anchor links would result in a browser refresh, which we don’t want. So instead, we can use `<Link>` to navigate to a particular URL and have the view re-rendered without a browser refresh.

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

>If the router’s path and the location are successfully matched, an object is created and we call it the match object


## How to implement a nested routing?

In the users.js file, we need to import the react **router** components because 
we need to implement the subroutes inside the Users Component.

```js
//users.js
import React from "react";
import { Route, Link, Switch, useRouteMatch } from "react-router-dom";
import User from "./user";

const Users = () => {
  let match = useRouteMatch();
  console.log(match);
  return (
    <div>
      <h1>Users</h1>
      <ul>
        <li>
          <Link to={`${match.url}/1`}>User 1 </Link>
        </li>
        <li>
          <Link to={`${match.url}/2`}>User 2 </Link>
        </li>
        <li>
          <Link to={`${match.url}/3`}>User 3 </Link>
        </li>
      </ul>
      <Switch>
        <Route path={`${match.path}/:id`}>
          <User />
        </Route>
        <Route path={match.path}>
          <h3>Please select a User.</h3>
        </Route>
      </Switch>
    </div>
  );
};
export default Users;
```

>First, we’ve declared a couple of links for the nested routes. As previously mentioned, match.url will be used for building nested links and match.path for nested routes.

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

## Protecting Routes

### Redirect

Like the server-side redirects, `<Redirect>` will replace the current location in the history stack with a new location. The new location is specified by the to prop. Here’s how we’ll be using `<Redirect>`:
```js
<Redirect to={{pathname: '/login', state: {from: props.location}}}
```

## Custom Route

A custom route is a fancy word for a route nested inside a component. If we need to make a decision whether a route should be rendered or not, writing a custom route is the way to go. Here’s the custom route declared among other routes.

fakeAuth.isAuthenticated returns true if the user is logged in and false otherwise.

Here’s the complete code:

```js
import Login, { fakeAuth } from "./Login";

export default function App() {
  return (
    <div>
      <nav className="navbar navbar">
        <ul className="nav navbar-nav">
          <li>
            <Link to="/">Homes</Link>
          </li>
          <li>
            <Link to="/category">Category</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
          </li>
          <li>
            <Link to="/admin">Admin area</Link>
          </li>
        </ul>
      </nav>

      <Switch>
        <Route path="/login" component={Login} />
        <Route exact path="/" component={Home} />
        <Route path="/category" component={Category} />
        <PrivateRoute path="/admin" component={Admin} />
        <Route path="/products" component={Products} />
      </Switch>
    </div>
  );
}

/* PrivateRoute component definition */
const PrivateRoute = ({ component: Component, ...rest }) => {
  return (
    <Route
      {...rest}
      render={props =>
        fakeAuth.isAuthenticated === true ? (
          <Component {...props} />
        ) : (
          <Redirect
            to={{ pathname: "/login", state: { from: props.location } }}
          />
        )
      }
    />
  );
};
```

The route renders the Admin component if the user is logged in. Otherwise, the user is redirected to /login. The good thing about this approach is that it is evidently more declarative and PrivateRoute is reusable.

[blog](https://www.sitepoint.com/react-router-complete-guide/)
[here](https://codesandbox.io/s/trusting-elgamal-dzd0s?file=/src/contact.js) is a sample application which I created while writing this note.


