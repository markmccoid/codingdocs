# App Creation

To create an app use the following:

```npm
	//Create the project -- this creates a directory of projectName
	react-native init projectName
	
	//Run the simulator
	react-native run-ios
```

## Initial App Setup
The entry points into a react native application are the *index.ios.js* and *index.andriod.js*.
The method Stephen Grider uses is to have both of these entries point be the same code. 

```javascript
	import { AppRegistry } from 'react-native';
	import App from './src/App';
	
	AppRegistry.registerComponent('manager', () => App);
```
The AppRegistry.registerComponent function has two arguments. The first must be the name you used when you ran the react-native init command. The next argument must be a function returning the a react component. Above, we are importing the App component from the src directory.

I believe that the first argument to the registerComponent function must be lower case and match the directory of the application.
The second function is redirecting to the root js file for the application.  Here we are using App.js as the root.

> NOTE: To get a view to scroll you need to put a style of flex: 1 on the top level view of the components you want to be scrollable:

```javascript
<View style={{ flex: 1 }}>
	<Header headerText='Albums!'/>
	<AlbumList />
</View>
```

## Boilerplate for a Redux application
### using react-native-router-flux for navigation
Below is the boilerplate code for an application using redux and navigation with [react-native-route-flux](https://github.com/aksonov/react-native-router-flux).

```javascript
	import React from 'react';
	import { View, Text } from 'react-native';
	import { Provider } from 'react-redux';
	import { createStore, applyMiddleware } from 'redux';
	import ReduxThunk from 'redux-thunk';
	import reducers from './reducers';
	import firebase from 'firebase';
	
	import Router from './Router';
	
	class App extends React.Component {
	  componentWillMount() {
	    // Initialize Firebase
	    const config = {
	      apiKey: 'AIzaSyBq6sapPRNHlRxaLU5UzwsnYpHJRnvwLMg',
	      authDomain: 'manager-642cb.firebaseapp.com',
	      databaseURL: 'https://manager-642cb.firebaseio.com',
	      storageBucket: 'manager-642cb.appspot.com',
	      messagingSenderId: '67690040094'
	    };
	    firebase.initializeApp(config);
	  }
	  render() {
	    const store = createStore(reducers, {}, applyMiddleware(ReduxThunk));
	    //store.subscribe(()=> console.log(store.getState()));
	    return (
	      <Provider store={store}>
	          <Router />
	      </Provider>
	    );
	  };
	}
	
	export default App;
```


#### Redux setup
This example is creating the redux store in the app code.  Note that the reducers import is a folder with an index file that is exporting all of the specific reducer JS files.
The reducer index file is using the combineReducers() function to export a reducer for the store to use:

```javascript
	import { combineReducers } from 'redux';
	import AuthReducer from './AuthReducer';
	import EmployeeFormReducer from './EmployeeFormReducer';
	
	export default combineReducers({
	  auth: AuthReducer,
	  employeeForm: EmployeeFormReducer
	});
```

#### Navigation with react-native-router-flux
In this app, we have store our routes (Scenes) in a separate **Router.js** file.
```javascript
	import React from 'react';
	import { Scene, Router } from 'react-native-router-flux';
	import LoginForm from './components/LoginForm';
	import EmployeeList from './components/EmployeeList';
	import EmployeeCreate from './components/EmployeeCreate';
	import { Actions } from 'react-native-router-flux';
	
	const RouterComponent = () => {
	
	  return (
	    <Router sceneStyle={{ paddingTop: 65}}>
	      <Scene key="auth">
	        <Scene key="login" component={LoginForm} title="Please login" />
	      </Scene>
	      <Scene key="main">
	        <Scene
	          initial
	          rightTitle="Add"
	          onRight={() => Actions.employeeCreate()}
	          key="employeeList"
	          component={EmployeeList}
	          title="Employees"
	        />
	        <Scene key="employeeCreate" component={EmployeeCreate} title="Create Employee" />
	      </Scene>
	    </Router>
	  );
	};
	
	export default RouterComponent;
```

The top level component for navigation with this component is **\<Router\>**.
The children of this component will be **\<Scene\>** components.  Each Scene is a view/separate screen in your application.
Scenes can have nested scenes.  The top level Scenes are useful for grouping scenes together allowing navigation back to a group.  However, when navigating to a group, only the first scene OR the scene with the key of **initial** set to true will show.
For scenes to display, there are three required keys, they are:
- **key** - this is the name that you will reference to in the Actions call to show the scene.
- **component** - this is the actual component that will be shown.
- **title** - this is the title of the scene.

**Other useful keys:**
- **rightTitle** - this will display a title link in the right side of the header.
- **onRight** - this is a function that will be called when the rightTitle is pressed.


## Common React Native Components

- **View** - Anything shown on the screen must be wrapped in a view
- **Text** - Text to display on the screen
- **TouchableOpacity** - Wraps a Text component and allows it to be pressed.
- **Modal**


# Config / App Secrets File

You should create a file outside of your components directory that will hold some basic configuration and items you want hidden from users.

Configuration would include device screen dimensions, api URLs, api Keys.

For Firebase, you could keep your firebase config and api information in a file like this.

Below is an example that get device dimensions.

```javascript
'use strict'

import Dimensions from 'Dimensions';

const window = Dimensions.get('window');

export default {
  windowHeight: window.height,
  windowWidth: window.width,

  windowHeightHalf: window.height * 0.5,
  windowHeightThird: window.height * 0.333,
  windowHeightTwoThirds: window.height * 0.666,
  windowHeightQuarter: window.height * 0.25,
  windowHeightFifth: window.height * 0.20,
  windowHeightThreeQuarters: window.height * 0.75,

  windowWidthHalf: window.width * 0.5,
  windowWidthThird: window.width * 0.333,
  windowWidthTwoThirds: window.width * 0.666,
  windowWidthQuarter: window.width * 0.25,
  windowWidthFifth: window.width * 0.20,
  windowWidthThreeQuarters: window.width * 0.75,
  windowWidthNineTenths: window.width * 0.9,

  apiUrl: 'https://newsapi.org/v1',
  apiKey: 'YOUR_API_KEY', // add your API key here

  /*
   * Create your own API key by signing up for newsapi.org: https://newsapi.org/
   * Plug it in here. We will explain how to use this in the next lesson
   */
}
```



# Third Party Components

## [react-native-communications](https://github.com/anarchicknight/react-native-communications)

Open a web address, easily call, email, text, iMessage (iOS only) someone in React Native

# React Native Playground

[Snack.io](https://snack.expo.io/)

This is a site that uses expo and allows you to test React Native code similar to codepen, jsbin, etc.

