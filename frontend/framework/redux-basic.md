## Contents

- [Core-Concepts](#core-concepts)
- [Principles](#principles)
- [Demo-Principles](#demo-principles)
- [Multiple-Reducers-Middleware](#multiple-reducers-middleware)
- [Async-Actions](#async-actions)
- [Thunk-Middleware](#thunk-middleware)
- [Folder-structure](#folder-structure)
- [My-Project](#my-project)

## Core-Concepts

- [Store] : Holds the state of your application
- [Action] : Describes what happened
- [Reducer] : Ties the store and actions together

---

## Principles

- [First] : **The state of your whole application is stored in an object tree within a single store**
- [Second] : **The only way to change the state is to emit an action, an object describing what happened** e.g., `{type: ADD_ITEM }`
- [Third] : **To specify how the state tree is transformed by actions, you write pure reducers** e.g., `Reducer - (prevousState, action) => newState`

![](/images/redux-01.png)

## Demo-Principles

```js
const redux = require('redux');
const createStore = redux.createStore;

const BUY_CAKE = 'BUY_CAKE';

// * action

function buyCake() {
  return {
    type: BUY_CAKE,
    info: 'First redux action',
  };
}

// ? reducer

const initialState = {
  numOfCakes: 10,
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case BUY_CAKE:
      return {
        ...state, //copy state then change what it need
        numOfCakes: state.numOfCakes - 1,
      };
    default:
      return state;
  }
};

// ! Store
// Holds application state
// Allows access to state via getState()
// allows state to be updated via dispatch(action)
// registers listeners via subscribe(listener)
// Handles unregistering of listeners via the function returned by subscribe(listener)

const store = createStore(reducer);

console.log('Initial state', store.getState());

//run whenever our state update
const unsubscribe = store.subscribe(() =>
  console.log('Updated state', store.getState())
);

store.dispatch(buyCake());
store.dispatch(buyCake());
store.dispatch(buyCake());

unsubscribe();
```

## Multiple-Reducers-Middleware

```js
const redux = require('redux');

const createStore = redux.createStore; //creating store
const combineReducers = redux.combineReducers; //combine our reducers

//middleware
const reduxLogger = require('redux-logger');
const logger = reduxLogger.createLogger();
const applyMiddleware = redux.applyMiddleware;

const BUY_CAKE = 'BUY_CAKE';
const BUY_ICECREAM = 'BUY_ICECREAM';

// * action

function buyCake() {
  return {
    type: BUY_CAKE,
    info: 'First redux action',
  };
}

function buyIceCream() {
  return {
    type: BUY_ICECREAM,
  };
}

// state

const initialCakeState = {
  numOfCakes: 10,
};

const initialIceCreamState = {
  numOfIceCreams: 20,
};

// Multiple reducers

const cakeReducer = (state = initialCakeState, action) => {
  switch (action.type) {
    case BUY_CAKE:
      return {
        ...state, //copy state then change what it need
        numOfCakes: state.numOfCakes - 1,
      };
    default:
      return state;
  }
};

const iceCreamReducer = (state = initialIceCreamState, action) => {
  switch (action.type) {
    case BUY_ICECREAM:
      return {
        ...state,
        numOfIceCreams: state.numOfIceCreams - 1,
      };
    default:
      return state;
  }
};

// root reducer
// accept a object with key value pair

const rootReducer = combineReducers({
  cake: cakeReducer,
  iceCream: iceCreamReducer,
});

// ! Store

const store = createStore(rootReducer, applyMiddleware(logger));

console.log('Initial state', store.getState());

//run whenever our state update
const unsubscribe = store.subscribe(() =>
  //  logger middleware console out the action and updated state
  {}
);

store.dispatch(buyCake());
store.dispatch(buyIceCream());
store.dispatch(buyCake());
store.dispatch(buyIceCream());
store.dispatch(buyCake());
store.dispatch(buyIceCream());

unsubscribe();
```

## Async-Actions

```js
// state
state = {
  loading: true,
  data: [],
  error: '',
};
```

**loading** - Display a loading spinner in your Component <br>
**data** - List of users <br>
**error** - Display error to the user <br>

### actions

**FETCH_USERS_REQUEST** - Fetch list of users <br>
**FETCH_USERS_SUCCESS** - Fetched successfully <br>
**FETCH_USERS_FAILURE** - Error fetching the data <br>

### reducers

- case: **FETCH_USERS_REQUEST** <br>
  loading: true <br>
- case: **FETCH_USERS_SUCCESS** <br>
  loading: false <br>
  users: data ( from API ) <br>
- case: **FETCH_USERS_FAILURE** <br>
  loading: false <br>
  error: error ( from API ) <br>

## Thunk-Middleware

Redux Thunk middleware allows you to write action creators that return a function instead of an action. The thunk can be used to delay the dispatch of an action, or to dispatch only if a certain condition is met. The **inner function receives the store methods `dispatch` and `getState` as parameters.**

```js
const redux = require('redux');
const createStore = redux.createStore;
// middleware
const applyMiddleware = redux.applyMiddleware;
const thunkMiddleware = require('redux-thunk').default;
const axios = require('axios');

const initialState = {
  loading: false,
  users: [],
  error: '',
};

const FETCH_USERS_REQUEST = 'FETCH_USERS_REQUEST';
const FETCH_USERS_SUCCESS = 'FETCH_USERS_SUCCESS';
const FETCH_USERS_FAILURE = 'FETCH_USERS_FAILURE';

//Actions

const fetchUsersRequest = () => {
  return {
    type: FETCH_USERS_REQUEST,
  };
};

const fetchUsersSuccess = (users) => {
  return {
    type: FETCH_USERS_SUCCESS,
    payload: users,
  };
};

const fetchUsersFailure = (error) => {
  return {
    type: FETCH_USERS_FAILURE,
    payload: error,
  };
};

//Reducers

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case FETCH_USERS_REQUEST:
      return {
        ...state,
        loading: true,
      };
    case FETCH_USERS_SUCCESS:
      return {
        loading: false,
        users: action.payload,
        error: '',
      };
    case FETCH_USERS_FAILURE:
      return {
        loading: false,
        users: [],
        error: action.payload,
      };
  }
};

// action creator

const fetchUsers = () => {
  return function (dispatch) {
    dispatch(fetchUsersRequest());
    axios
      .get('https://jsonplaceholder.typicode.com/users')
      .then((response) => {
        // reponse.data is the array of users
        const users = response.data.map((user) => user.id);
        dispatch(fetchUsersSuccess(users));
      })
      .catch((error) => {
        // error.message is the error description
        dispatch(fetchUsersFailure(error.message));
      });
  };
};

//store

const store = createStore(reducer, applyMiddleware(thunkMiddleware));
store.subscribe(() => {
  console.log(store.getState());
});
store.dispatch(fetchUsers());
```

## Folder-structure

    redux
    │   root-reducer.js
    │   store.js
    │
    └───user
    │   │   user.actions.js
    │   │   user.reducer.js

---

## My-Project

### root-reducer.js:

```js
import { combineReducers } from 'redux';
import userReducer from './user/user.reducer';

export default combineReducers({
  user: userReducer,
});
```

### store.js:

```js
import { createStore, applyMiddleware } from 'redux';
import logger from 'redux-logger';

import rootReducer from './root-reducer';

const middlewares = [logger];

const store = createStore(rootReducer, applyMiddleware(...middlewares));

export default store;
```

### user.actions.js:

```js
export const setCurrentUser = (user) => ({
  type: 'SET_CURRENT_USER',
  payload: user,
});
```

### user.reducer.js:

```js
const INITIAL_STATE = {
  currentUser: null,
};
const userReducer = (state = INITIAL_STATE, action) => {
  switch (action.type) {
    case 'SET_CURRENT_USER':
      return {
        ...state,
        currentUser: action.payload,
      };
    default:
      return state;
  }
};

export default userReducer;
```

In order to pass data to other components we need to rewrite our logic in `app.js` Before that we need to pass the store to our app.js via `index.js`

```js
<Provider store={store}>
  <BrowserRouter>
    <App />
  </BrowserRouter>
</Provider>
```

Then our `app.js` get store as props:

```js
import { connect } from 'react-redux';
import { setCurrentUser } from  './redux/user/user.actions';
class App extends React.Component {
  unsubscribeFromAuth = null;
  componentDidMount() {
    const { setCurrentUser } = this.props;
    this.unsubscribeFromAuth = auth.onAuthStateChanged(async userAuth => {
      if (userAuth) {
        const userRef = await createUserProfileDocument(userAuth);
        userRef.onSnapshot(snapShot => {
          setCurrentUser({
              id: snapShot.id,
              ...snapShot.data()
            })
        })
      }

        setCurrentUser(userAuth);

    })
  }
  componentWillUnmount() {
    this.unsubscribeFromAuth();
  }

  render () { ... }
}
const mapDispatchToProps = dispatch => ({
  setCurrentUser: user => dispatch(setCurrentUser(user))
})
export default connect(null,mapDispatchToProps)(App);
```

Now have a look how we can use the state in other component(like Header comp.):

```js
import { connect } from 'react-redux';
const Header = ({ currentUser }) => ( ... )
const mapStateToProps = (state) => ({
    currentUser: state.user.currentUser
});
export default connect(mapStateToProps)(Header);
```

---
