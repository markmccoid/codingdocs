## User Authentication in Firebase

First and foremost, see the [Firebase Docs](/javascript/firebase) to make sure you have the correct Authentication Rules set up and that you have set up a way for your user to "Login" to your application.  

Once you have this set up, you will probably have the root of your app "/" be your login in page.  After the user logs in you will need to redirect him.  This can be accomplished in the **app.js** or root js file for your application.

```javascript
import React ...
...
import AppRouter, { history } from './routers/AppRouter';
import { firebase } from './firebase/firebase';
...

const jsx = (
  <Provider store={store}>
    <AppRouter />
  </Provider>
);
let hasRendered = false;
const renderApp = () => {
  if (!hasRendered) {
    ReactDOM.render(jsx, document.getElementById('app'));
    hasRendered = true;
  }
};

firebase.auth().onAuthStateChanged((user) => {
  if (user) {
    console.log(`logged in as ${user.uid}`);
    store.dispatch(expenseActions.startSetExpenses()).then(() => {
      renderApp();
      if (history.location.pathname === '/') {
        history.push('/dashboard');
      }
    });
  } else {
    renderApp();
    history.push('/');
  }
});
```

The flow of this, will be a user comes to your site and is presented with the login page.  They go through the login process and once logged in the following happens:

1. Redux action is dispatched to update user.uid in the redux store.
2. Any initial data is fetched
3. User is redirected to page

Here is an example set of action creators for the login and logout actions.  Note, that we need to enhance this for error catching and use of multiple auth providers (at least email).

```javascript
import { firebase, googleAuthProvider } from '../firebase/firebase';

export const login = uid => ({
  type: 'LOGIN',
  uid
});

export const startLogin = () => {
  return (dispatch) => {
    return firebase.auth().signInWithPopup(googleAuthProvider);
  };
};

export const logout = () => ({
  type: 'LOGOUT'
});
export const startLogout = () => {
  return (dispatch) => {
    firebase.auth().signOut();
  };
};

```

Here is the auth reducer:

```javascript
export default (state = {}, action) => {
  switch (action.type) {
    case 'LOGIN':
      return {
        uid: action.uid
      };
    case 'LOGOUT':
      return {};
    default:
      return state;
  }
}
```

