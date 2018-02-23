# My Redux Pattern

There are a number of ways to set up your folder structure for redux, with the most basic simply being a folder for ActionTypes, Actions, Reducers and Selectors.

However, as your data model gets more complex these files get crowded.  I initially tried creating a separate filename for each node in my data model under the Actions, Reducers, etc folders, but you find yourself hopping around a lot.

Then I found the "Ducks" pattern, whose goal is to encapsulate much of this in a single file for each area of functionality.  

I opted for a modified version of ducks based on this article: [Scaling Your Redux App With Ducks](https://medium.freecodecamp.org/scaling-your-redux-app-with-ducks-6115955638be)

 Here is the [original ducks proposal](https://github.com/erikras/ducks-modular-redux)

Below is a screenshot of what my folder structure looks like:

![](https://cl.ly/3w0P103W3f1F/Image%202018-02-23%20at%203.09.12%20PM.png)

You can see most *redux*  functionality is in the **store** folder.  

At the root we have the *configureStore.js* file.  This will do our initial setup. 

```javascript
import { createStore, combineReducers, applyMiddleware, compose } from 'redux';
import { AUTH_LOGGED_OUT, startLogin } from './auth';
import thunk from 'redux-thunk';
import logger from 'redux-logger';
// *** We are pulling the reducers as defaults from the index files in each directory
import tvShowReducer from './tvShows';
import authReducer from './auth';

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const initialState = {
  TV: {
    showData: {},
    seasonData: {},
    extraData: {}
  },
  auth: {
    uid: undefined,
    status: AUTH_LOGGED_OUT,
    msg: undefined
  }
}
export default () => {
  //store creation
  const store = createStore(
    combineReducers({
      TV: tvShowReducer,
      auth: authReducer,
    }),
    initialState,
    composeEnhancers(applyMiddleware(thunk, logger))
  );
  return store;
};

```

Each of the folders under **store** holds all redux activity for a node in the data model.

So the **tvShows** folder exports the reducer for the **TV:** node.

Let's go through each file in the sub folders.

## Actions.js

This file will hold the Action Type constants and all of the Action Creators.  These will all be named exports:

```javascript
export const LOGIN = 'LOGIN'
export const startLogin = () => {}
```

## Reducers.js

This file will hold all of the reducers.  You can also use combineReducers in this file if there are sub nodes in the data model.

For example our data shape is

```javascript
{
    TV: {
        showData: {},
        seasonData: {}
    }
}
```

With the above, in createStore we would do this:

```javascript
import tvShowReducer from './tvShows';

export default () => {
  //store creation
  const store = createStore(
    combineReducers({
      TV: tvShowReducer, // <----
      auth: authReducer,
    }),
    initialState,
    composeEnhancers(applyMiddleware(thunk, logger))
  );
  return store;
};
```

But then in our Reducers file we would have created two separate reducer functions and used combineReducers on them before exporting.

```javascript
import { combineReducers } from 'redux';

// -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
// - showData reducer
// -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
const showDataDefault = {};
const showDataReducer = (state = showDataDefault, action) => {
  switch (action.type) {
    case ADD_TV_SHOW: 
      return ...
    case DELETE_TV_SHOW: 
	  return ...
 	}
    default:
      return state;
  }
};

// -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
// - seasonData reducer
// -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
const seasonDataDefault = {};
const seasonDataReducer = (state = seasonDataDefault, action) => {
  switch (action.type) {
    case ADD_TV_SHOW: 
      return ...
    case DELETE_TV_SHOW: 
	  return ...
 	}
    default:
      return state;
  }
};

const reducer = combineReducers({
  showData: showDataReducer,
  seasonData: seasonDataReducer,
});

export default reducer;
```

We will always `export default reducer` 

## selectors.js

Selectors.js will contain all of the functions we need to slice and dice the data for this node.

All of these are also named exports.

## index.js

This file is very important as it allows us to import all of these functions and constants easily.  It will look the same for every functional directory:

```javascript
import reducer from './reducers';

export * from './actions';
export * from './selectors';

export default reducer;
```

Here is how you would import and access your reducers, actions and selectors.

```javascript
import tvShowReducer from './store/tvShows' //<-- default export so just access directory 

import { ACTION_TYPE } from './store/tvShows' //<-- Named export accessed by name
import { startAddTVShow } from './store/tvShows'//<-- Named export accessed by name
import { selectSomeData } from './store/tvShows'//<-- Named export accessed by name
```

