## User Authentication in Firebase

First and foremost, see the [Firebase Docs](/javascript/firebase) to make sure you have the correct Authentication Rules set up and that you have set up a way for your user to "Login" to your application.  

### Setup App.js onAuthStateChanged 

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

### Flow of User Authentication in App

The flow of this, will be a user comes to your site and is presented with the login page.  They go through the login process and once logged in the following happens:

1. Redux action is dispatched to update user.uid in the redux store.
2. Any initial data is fetched
3. User is redirected to page
4. Make Private pages. There will be pages that cannot be viewed unless logged in. Make sure these pages are locked down.
5. Make Public pages.  There will be pages that are public but shouldn't be seen once logged in. Like the login page.

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

### Make Private and Public Pages

Probably a couple of ways to do this.  One would be an HOC which would check to see if the user was logged in and would either return the passed in Component if logged in or redirect back to the Login page.

The other way is to create a component that encapsulates React Routers Route component.

Here is our **AppRoute.js** file:

```javascript
import React from 'react';
import { Router, Route, Switch } from 'react-router-dom';
import createHistory from 'history/createBrowserHistory';

import Login from '../components/Login';
import ExpenseDashboardPage from '../components/ExpenseDashboardPage'; 
import EditExpensePage from '../components/EditExpensePage'; 
import AddExpensePage from '../components/AddExpensePage'; 
import HelpPage from '../components/HelpPage';
import NotFound from '../components/NotFound';
import PrivateRoute from './PrivateRoute';

export const history = createHistory();

const AppRouter = () => (
  <Router history={history}>
    <div>
      <Switch>
        <Route exact path="/" component={Login} />
        <PrivateRoute path="/dashboard" component={ExpenseDashboardPage} />
        <PrivateRoute path="/create" component={AddExpensePage} />
        <PrivateRoute path="/edit/:id" component={EditExpensePage} />
        <Route path="/help" component={HelpPage} />
        <Route component={NotFound} />
      </Switch>
    </div>
  </Router>
);

export default AppRouter;
```

Here is the PrivateRoute component that does the work.  It connects to the redux store and determines if the user is logged in.  If they are then it returns a Route component setup properly.  If not logged in, then it redirects to the login page, which is "/" in this example.

```javascript
import React from 'react';
import { connect } from 'react-redux';
import { Route, Redirect } from 'react-router-dom';
import Header from '../components/Header';

//Destructure the props, get isAuthenticated from redux connect
//Grab the component renaming to Component so that React sees it as a component
//then use the rest operator to grab the rest of the props
export const PrivateRoute = ({
  isAuthenticated,
  component: Component,
  ...rest
}) => (
  <Route
    {...rest}
    component={props => (
      isAuthenticated ? (
        [<Header />,
          <Component {...props} />
        ]
      ) : (
        <Redirect to="/" />
      )
    )
  }
  />
);

const mapStateToProps = state => ({
  isAuthenticated: !!state.auth.uid
});

export default connect(mapStateToProps)(PrivateRoute);
```

You can do the same thing if you want to have some pages not able to be navigated to when logged in.  We could call this implementation the PublicRoute:

```javascript
import React from 'react';
import { connect } from 'react-redux';
import { Route, Redirect } from 'react-router-dom';

export const PublicRoute = ({
  isAuthenticated,
  component: Component,
  ...rest
}) => (
  <Route
    {...rest}
    component={props => (
      !isAuthenticated ? (
        <Component {...props} />
      ) : (
        <Redirect to="/dashboard" />
      )
    )
  }
  />
);

const mapStateToProps = state => ({
  isAuthenticated: !!state.auth.uid
});

export default connect(mapStateToProps)(PublicRoute);

```

### Accessing only Logged in User Area in Firebase

You can handle this in your Action Creators since you will be using a thunk.  The thunk's signature is ` (dispatch, getState)`.  So you will have access to the state:

```javascript
export const startAddExpense = (expenseData = {}) => {
  return (dispatch, getState) => {
    //Get the uid from the store
    const { uid } = getState().auth;
    const {
      description,
      note,
      amount,
      createdAt
    } = expenseData;
    const expense = { description, note, amount, createdAt, };
    //Use the uid to access the correct users expenses
    return database.ref(`users/${uid}/expenses`).push(expense)
      .then((ref) => {
        dispatch(addExpense({
          id: ref.key,
          ...expense
        }));
      });
  };
};
```

