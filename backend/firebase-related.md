## contents:

- [firestore.doc](https://firebase.google.com/docs/firestore/query-data/get-data)
- [QueryReference](#queryreference)
- [DocumentReference-vs-CollectionReference](#documentreference-vs-collectionreference)
- [DocumentSnapshot](#documentsnapshot)
- [QuerySnapshot](#querysnapshot)
- [My-Project](#my-project)

# QueryReference and QuerySnapshot

# QueryReference

A query is a request we make to firestore to give us something from the database.

Firestore returns us two type of objects: references and snapshots. Of these objects, they can be either Document or Collection versions.

Firestore will always return us these objects, even if nothing exists at from that query.

A queryReference object is an object that represents the **current** place in the database that we are querying.

we get them by calling either:

```js
firestore.doc(`/users/:userId`);
firestore.collections(`/users`);
```

The **queryReference object does not have the actual data of the collection or document**. It instead has the properties that tell us details about it, or the method to get the Snapshot object which gives us the data we are looking for.

## DocumentReference-vs-CollectionReference

we use documentRef objects to perform our CURD methods (create, retrieve, update, delete). The documentRef methods are **.set()**, **.get()**, **.update()** and **.delete()** respectively.

we can also add documents to collections using the collectionRef object using the **.add()** method. `collectionRef.add( {value:prop} )`

we get the snapshotObject from the referenceObject using the **.get()** method. ie. documentRef.get() or collectionRef.get()

documentRef returns a documentSnapshot object. collectionRef returns a querySnapshot object. One more method is **onSnapshot** which create a snapshot listener so that whenever the snapshot changes right meaning that whenever the documents snapshot object updates whether because we set a new value or we updated the value or we deleted the value it will pass that snapshot into our listener which we've attached to this `onSnapshot` So our redux method to set the actual current State.

## DocumentSnapshot

we get a documentSnapshot object from our documentReference object. The documentSnapshot object allows us to check if a document exists at this query using the **.exists** property which returns a boolean.

we can also get the actual properties on the object by calling the **.data()** method, which returns us a JSON object of the document.

## QuerySnapshot

we get a querySnapshot object from our collectionReference object.

we can check if there are any documents in the collection by calling the **.empty** property which returns a boolean.

we can get all the documents in the collection by calling the **.docs** property. It returns an array of our documents as documentSnapshot objects.

## My-Project

auth.onAuthStateChanged and onSnapshot are like an observable

![](/images/firebase-subscription.png)

we can alternatively resolve this in our `componentDidMount() {...}` using `fetch` or `promise`: <br>
This can be replaced using native **fetch:** <br>

```js
fetch(
  'https://firestore.googleapis.com/v1/projects/crwn-db-f5d1c/databases/(default)/documents/collections'
)
  .then((response) => response.json())
  .then((collections) => console.log(collections));
```

or using **promise:** <br>

```js
collectionRef.get().then((snapshot) => {
  const collectionsMap = convertCollectionsSnapshotToMap(snapshot);
  updateCollections(collectionsMap);
  this.setState({ loading: false });
});
```

## Create firebase.utils.js:

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

## Handling logic inside app.js:

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
      currentUser: null,
    };
  }

  unsubscribeFromAuth = null;

  componentDidMount() {
    this.unsubscribeFromAuth = auth.onAuthStateChanged(async (userAuth) => {
      if (userAuth) {
        const userRef = await createUserProfileDocument(userAuth);

        userRef.onSnapshot((snapShot) => {
          this.setState({
            currentUser: {
              id: snapShot.id,
              ...snapShot.data(),
            },
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
