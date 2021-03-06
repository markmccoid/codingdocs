# React Native Debugging 

I was unable to get redux dev tools to work for me in Chrome when using **create-react-native-app**.

What ended up working for me was setting up and using **[React Native Debugger](https://github.com/jhen0409/react-native-debugger)**

Here is a article on setting this up.  I found that the *postinstall* script didn't work in create-react-native-app.

[Develop React Native Apps Like a Boss](https://medium.com/react-native-development/develop-react-native-redux-applications-like-a-boss-with-this-tool-ec84bed7af8)

### Install React Native Debugger

React Native Debugger can be installed as follows:

```bash
$ brew update && brew cask install react-native-debugger
```

This will actually install an application in your applications folder on the mac.

### Setup Redux

I had trouble with the following, but it is now working.  The below is the proper way, but if it doesn't work, you can look at the other set up below this one.

```javascript
import { createStore, combineReducers, applyMiddleware, compose } from 'redux';
// import { composeWithDevTools } from 'remote-redux-devtools';
import thunk from 'redux-thunk';
import { createLogger } from 'redux-logger';

import carsReducer from '../reducers/cars';
import servicesReducer from '../reducers/services';
import filtersReducer from '../reducers/filters';
import authReducer from '../reducers/auth';

const loggerMiddleware = createLogger({ predicate: (getState, action) => __DEV__ });

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({
    // Prevents Redux DevTools from re-dispatching all previous actions.
    shouldHotReload: false
  }) || compose;

const enhancer = composeEnhancers(applyMiddleware(thunk, loggerMiddleware));

export default () => {
  //store creation
  const store = createStore(
    combineReducers({
      cars: carsReducer,
      services: servicesReducer,
      filters: filtersReducer,
      auth: authReducer,
    }),
    enhancer
  );
  
  return store;
};
```



Here is a createStore function that injects what is needed for the remote debugger:

```Javascript
import { createStore, combineReducers, applyMiddleware, compose } from 'redux';
// import { composeWithDevTools } from 'remote-redux-devtools';
import thunk from 'redux-thunk';
import { createLogger } from 'redux-logger';

import carsReducer from '../reducers/cars';
import servicesReducer from '../reducers/services';
import filtersReducer from '../reducers/filters';
import authReducer from '../reducers/auth';

const loggerMiddleware = createLogger({ predicate: (getState, action) => __DEV__ });

const enhancer = compose(
  applyMiddleware(thunk, loggerMiddleware),
  global.reduxNativeDevTools ?
    global.reduxNativeDevTools() :
    noop => noop
);

export default () => {
  //store creation
  const store = createStore(
    combineReducers({
      cars: carsReducer,
      services: servicesReducer,
      filters: filtersReducer,
      auth: authReducer,
    }),
    enhancer
  );

  if(global.reduxNativeDevTools) {
    global.reduxNativeDevTools.updateStore(store);
  }
  return store;
};
```

Note, the global.reduxNativeDevTools lines.  These are the ones setting up what is need for using **React Native Debugger**

Another option for setting up redux can be found in this udemy course section:

[Redux Debugger Option 2](https://www.udemy.com/react-native-the-practical-guide/learn/v4/t/lecture/8567960?start=0)

### Using the Debugger

Ok, so setting it up was pretty easy, but now using it is a bit tricky.  The below assumes you are using create-react-native-app and expo, which is why we need to start the debugger on a specific port.

1. Start React Native Debugger 
   `$ open "rndebugger://set-debugger-loc?host=localhost&port=19001"`
2. Run `$ yarn start` in your applications directory
3. When the simulator starts, choose to debug remotely.  Make sure no other debug sessions are open.

