course-git-repo : https://github.com/kaloraat/react-node-next-multi-user-blogging-platform/

## dependencies info:

- **reactstrap:** for avoid jQuery in bootstrap we use reactstrap
- **isomorphic-fetch:** [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) is implementation of fetch for both node.js and browser, built on top of [fetch](https://github.com/github/fetch) polyfill.
- **js-cookie:** A simple, lightweight JavaScript API for handling cookies
- **nprogress:** progress bar at top
- **react-quill:** Quill is a free, open-source WYSIWYG editor built for the modern web. With its extensible architecture and an expressive API, you can completely customize it to fulfill your needs
- **moment:** Parse, validate, manipulate, and display dates and times in JavaScript
- **react-render-html:** Render HTML as React element, possibly replacing dangerouslySetInnerHTML

## Initial files + command:

- next.config.js

```js
module.exports = {
  publicRuntimeConfig: {
    APP_NAME: "SEOBLOG",
    API_DEVELOPMENT: "http://localhost:8000/api",
    PRODUCTION: false
  }
};
```

- config.js

```js
import getConfig from "next/config";
const { publicRuntimeConfig } = getConfig();

export const API = publicRuntimeConfig.PRODUCTION
  ? "https://seoblog.com"
  : "http://localhost:8000";
export const APP_NAME = publicRuntimeConfig.APP_NAME;
```

## Authentication

---

<center> <h1>SignUp</h1> </center>
SignupComponent.js:

```js
import { useState } from "react";
import { signup } from "../../actions/auth";

const SignupComponent = () => {
  const [values, setValues] = useState({//initial_data});

  const { name, email, password, error, loading, message, showForm } = values;

  const handleSubmit = e => {
    e.preventDefault();
    setValues({ ...values, loading: true, error: false });
    const user = { name, email, password };

    signup(user).then(data => {
      if (data.error) {
        setValues({ ...values, error: data.error, loading: false });
      } else {
        setValues({//reset_data});
      }
    });
  };

  const handleChange = name => e => {
    setValues({ ...values, error: false, [name]: e.target.value });
  };

  const showLoading = () =>
    loading ? <div className="alert alert-info">Loading...</div> : "";
  const showError = () =>
    error ? <div className="alert alert-danger">{error}</div> : "";
  const showMessage = () =>
    message ? <div className="alert alert-info">{message}</div> : "";

  const signupForm = () => {
    return <form onSubmit={handleSubmit}></form>;
  };

  return (
    <React.Fragment>
      {showError()}
      {showLoading()}
      {showMessage()}
      {showForm && signupForm()}
    </React.Fragment>
  );
};

export default SignupComponent;
```

---

<center> <h1>SignIn</h1> </center>
SigninComponent.js:

```js
signin(user).then(data => {
  if (data.error) {
    setValues({ ...values, error: data.error, loading: false });
  } else {
    // save user token to cookie
    // save user info to localstorage
    // authenticate user
    console.table(data);
    Router.push(`/`);
  }
});
```

---

auth.js:

```js
import fetch from "isomorphic-fetch";
import { API } from "../config";
import cookie from "js-cookie";

export const signup = user => {
  return fetch(`${API}/signup`, {
    method: "POST",
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json"
    },
    body: JSON.stringify(user)
  })
    .then(response => {
      return response.json();
    })
    .catch(err => console.log(err));
};
// set cookie: cookie.set(key, value[,options]) & cookie.remove(key[,])
export const setCookie = (key, value) => {...}
export const removeCookie = key => {...}
// get cookie: cookie.get(key)
export const getCookie = key => {...}
// localstorage: localStorage.setItem(key, JSON.stringify(value))
export const setLocalStorage = (key, value) => {...}
export const removeLocalStorage = key => {...}
// autheticate user by pass data to cookie and localstorage
export const authenticate = (data, next) => {
  setCookie("token", data.token);
  setLocalStorage("user", data.user);
  next();
};
export const isAuth = () => {
  if (process.browser) {
    const cookieChecked = getCookie("token");
    if (cookieChecked) {
      if (localStorage.getItem("user")) {
        return JSON.parse(localStorage.getItem("user"));
      } else {
        return false;
      }
    }
  }
};
```

signout didn't a Component at all. It just a method invoke.

## Extra information:

During creating this project

- **FormData:** The [FormData](https://www.youtube.com/watch?v=9mhyo1wQGeI) interface provides a way to easily construct a set of key/value pairs representing form fields and their values, which can then be easily sent using the XMLHttpRequest.send() method. It uses the same format a form would use if the encoding type were set to "multipart/form-data"
- **Puppeteer:** Web scrapping / Web automation
- **Fastify:** Creating API - REST
- **Next.js:** Server-side rendering
- **onChange:** A synthetic [event](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events) of VirtualDOM
- **[event processing](https://javascript.info/bubbling-and-capturing):**

  1. <mark>bubbling:</mark> The process is called “bubbling”, because events “bubble” from the inner element up through parents like a bubble in the water.
  2. <mark>capturing:</mark> With capturing, the event is first captured by the outermost element and propagated to the inner elements.

  > trickle down, bubble up

- **setState:** is async action/event `setState(updater[,callback])` where `updater=(state, props) => stateChange`
- **JSON:**

  1. <mark>JSON.stringify</mark> turns a JavaScript object into JSON text and stores that JSON text in a string
  2. <mark>JSON.parse</mark> turns a string of JSON text into a JavaScript object
  3. when you fetch data with POST method then if your data is in json format then use `'Content-Type': 'application/json' ... body: JSON.stringify(data)` and if it is form data then you can skip it `body: data`.

- **Router:** In nutshell - `history + store (redux) → react-router-redux`
  [react-router-dom](https://www.freecodecamp.org/news/a-complete-beginners-guide-to-react-router-include-router-hooks/) is a react-router plus:
  `<BrowserRouter> which is <Router history={browserNativeHistoryApiWrapper}/>`

- use .replace instead of .push simply when you don't want the browser to record any invalid url you have visited.

- Adding tag+category functionality in backend follow

  1. create model/Schema
  2. create validator
  3. create route
  4. create controller
  5. add route to server.js
  6. try using postman

- Adding tag+category functionality in fronend follow

  1. pages/admin/index >>> add link to 'admin/crud/category-tag' page
  2. action/tag >>> create/getTags/singleTag/removeTag
  3. components/crud/Tag >>> newTagForm/showTags/deleteTag/messages

- **D3.js:** [tutorial](http://curran.github.io/screencasts/introToD3/examples/viewer/#/)
- signin+signup+category.component.js have it's own state to handling fetching/showing/removing it's state/component
