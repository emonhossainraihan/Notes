## Source-list 
[firestore.doc](https://firebase.google.com/docs/firestore/query-data/get-data)
# Create firebase.utils.js:
```js
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';

const config = {...};

firebase.initializeApp(config);

export const createUserProfileDocument = async (userAuth, additionalData) => {
  if (!userAuth) return;

  const userRef = firestore.doc(`users/${userAuth.uid}`);

  const snapShot = await userRef.get();

  if (!snapShot.exists) {
    const { displayName, email } = userAuth;
    const createdAt = new Date();
    try {
      await userRef.set({
        displayName,
        email,
        createdAt,
        ...additionalData
      });
    } catch (error) {
      console.log('error creating user', error.message);
    }
  }

  return userRef;
};

export const auth = firebase.auth();
export const firestore = firebase.firestore();

const provider = new firebase.auth.GoogleAuthProvider();
provider.setCustomParameters({ prompt: 'select_account' });
export const signInWithGoogle = () => auth.signInWithPopup(provider);

export default firebase;  
```

# Handling logic inside app.js: 
## clear-cut:
As soon as a new Firebase account is created, you’re in a logged-in state and user’s auth object(userAuth in app.js) is available inside onAuthStateChange() method.<br>
[onSnapshot](https://firebase.google.com/docs/firestore/query-data/listen)<br>
Using real-time sync we can access the currentUser data within our entire application.  
    
```js
import React from 'react';
import { Switch, Route } from 'react-router-dom';

import SignInAndSignUpPage from './pages/sign-in-and-sign-up/sign-in-and-sign-up.component';

import { auth, createUserProfileDocument } from './firebase/firebase.utils';

class App extends React.Component {
  constructor() {
    super();

    this.state = {
      currentUser: null
    };
  }

  unsubscribeFromAuth = null;

  componentDidMount() {
    this.unsubscribeFromAuth = auth.onAuthStateChanged(async userAuth => {
      if (userAuth) {
        const userRef = await createUserProfileDocument(userAuth);

        userRef.onSnapshot(snapShot => {
          this.setState({
            currentUser: {
              id: snapShot.id,
              ...snapShot.data()
            }
          });

          console.log(this.state);
        });
      }

      this.setState({ currentUser: userAuth });
    });
  }

  componentWillUnmount() {
    this.unsubscribeFromAuth();
  }

  render() {
    return (
      <div>
        <Header currentUser={this.state.currentUser} />
        <Switch> ... </Switch>
      </div>
    );
  }
}

export default App;
```
