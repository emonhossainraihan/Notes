# Redux : basic setup 


    redux
    │   root-reducer.js
    │   store.js
    │
    └───user
    │   │   user.actions.js
    │   │   user.reducer.js
## root-reducer.js:

    import { combineReducers } from 'redux';
    import userReducer from './user/user.reducer';
    
    export default combineReducers({
        user: userReducer
    });
## store.js:

    import { createStore, applyMiddleware } from 'redux';
    import logger from 'redux-logger';
    
    import rootReducer from './root-reducer';
    
    const middlewares = [logger];
    
    const store = createStore(rootReducer,applyMiddleware(...middlewares));
    
    export default store;
## user.actions.js:
    export const setCurrentUser = user => ({
        type: 'SET_CURRENT_USER',
        payload: user
    })
## user.reducer.js:
    const INITIAL_STATE = {
        currentUser: null
    }
    const userReducer = (state = INITIAL_STATE, action) => {
        switch (action.type) {
            case 'SET_CURRENT_USER':
                return {
                    ...state,
                    currentUser: action.payload
                }
            default:
                return state;
        }
    }
    
    export default userReducer;
In order to pass data to other components we need to rewrite our logic in `app.js` Before that we need to pass the store to our app.js via `index.js`
    
    <Provider store={store}>
        <BrowserRouter>
            <App />
        </BrowserRouter>
    </Provider>
    
Then our `app.js` get store as props: 

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

Now have a look how we can use the state in other component(like Header comp.):

    import { connect } from 'react-redux';
    const Header = ({ currentUser }) => ( ... )
    const mapStateToProps = (state) => ({
        currentUser: state.user.currentUser
    });
    export default connect(mapStateToProps)(Header);
