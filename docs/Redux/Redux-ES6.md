## Intro
The main parts of the redux flow for react are:

3. &lt;Provider&gt;&lt;/Provider&gt; component
4. The store
5. Middleware - applyMiddleware()
6. actions and their Action Creators
7. reducers
8. The connect function

Very similar to ES5 syntax, but a bit different when using the connect() function.

## Provider Component
The Provider component is the wrapper for our application, it will wrap react-router also:

	import { Provider } from 'react-redux';
	...
	 <Provider store={createStoreWithMiddleware(reducers)}>
	 <Router history={browserHistory} routes={routes}/>
	 </Provider>

The store is where all the data will be stored. I have seen a couple of ways to create the store.

Here is one way, it is done in the main index.js file:

```javascript
	import React from 'react';
	import ReactDOM from 'react-dom';
	import { Provider } from 'react-redux';
	import { createStore, applyMiddleware } from 'redux';
	import { Router, browserHistory } from 'react-router';
	import promise from 'redux-promise';
	
	import reducers from './reducers';
	import routes from './routes';
	
	const createStoreWithMiddleware = applyMiddleware(promise)(createStore);
	
	ReactDOM.render(
	 <Provider store={createStoreWithMiddleware(reducers)}>
	 <Router history={browserHistory} routes={routes}/>
	 </Provider>
	 , document.querySelector('.container'));
```

## The Store

On line 11, we are creating a variable "createStoreWithMiddleware" which is setting up our createStore() function, but also passing "promise" (redux-promise), which greatly helps when pulling data using axios.

Then in the Provider component, we create the store and pass is to the store prop.

## connect() funtion

The connect() function allows you to inject your redux state into the components props object as we'll as map your action creators so that they are automatically send to the dispatch function.

We do the above two things by creating a mapStateToProps(state) function and mapDispatchToProps(dispatch) funciton.

```javascript
	function mapStateToProps(state) {
		return {
			weather: state.weather
		};
	}
	
	function mapDispatchToProps(dispatch) {
		return bindActionCreators({	fetchWeather }, dispatch);
	}
	
	export default connect(mapStateToProps, mapDispatchToProps)(SearchBar);
```

### mapStateToProps(state)

The state parameter is passed the full application state. We can then either return all of the state:

```javascript
	function mapStateToProps(state) {
		return state;
	}
```

Or, most likely just return the portion of state that this component/container needs. 

In that case, you will return an object where the key is the prop name and the value is the piece of state that you want in the component. 

> Note that the prop name (key) you choose, can be anything.

### mapDispatchToProps(dispatch) — See how to get rid of using this function.

This function creates new wrapper functions that will call the dispatch function with your action create as a parameter.
The function returns the bindActionsCreators() function, which is passed an object defining the new wrapped functions. 
It is easiest to simply use the ES6 object syntax and just pass the original action creator function name. 
This creates a wrapped function of the same name that you call via this.props.actionCreator(), which translates to dispatch(actionCreator).

The second parameter to bindActionCreators is the dispatch function that is passed as a parameter to the mapDispatchToProps function.

## export component using the connect() HOC

The last step is to export your component/container so that other components can have access.

This is done using the connect() function.

```javascript
	export default connect(mapStateToProps, mapDispatchToProps)(SearchBar);
	//Or if you do not need any application state
	export default connect(null, mapDispatchToProps)(SearchBar);
```
## Redux Shorthands for map functions

There are a few shortcuts. One will get rid of the need for the mapDispatchToProps function.

To do this, you simply pass the action creator functions that you want to "bind" to the dispatch function as an object to the second parameter to the connect() function:

```javascript
	export default connect(null, {fetchPosts: fetchPosts})(PostsIndex);
```

## Redux Helper Modules - i.e. Redux Middleware

There are a couple of helper modules specifically for Redux that I have found useful.

### Redux-Promise

If it receives a promise, it will dispatch the resolved value of the promise. It will not dispatch anything if the promise rejects.

This makes it so that we do not have to deal with the promise, it will only dispatch the action if and when the promise resolves.

**redux thunk** can also deal with promises as well as a number of other situations.  

 **Setup Example:** 

```javascript
	...
	import promise from 'redux-promise';
	
	const createStoreWithMiddleware = applyMiddleware(promise)(createStore);
	
	ReactDOM.render(
	 <Provider store={createStoreWithMiddleware(reducers)}>
	 <Router history={browserHistory} routes={routes}/>
	 </Provider>
	 , document.querySelector('.container'));

Action Creator example:

	export function createPost(props) {
		const request = axios.post(`${ROOT_URL}/posts${API_KEY}`, props);
	
		return {
			type: CREATE_POST,
			payload:request
		};
	}
```
When the above action creator is called, it will only dispatch to the reducers if and when the promise resolves.

However, wherever you call the action creator from will still return the promise if you need to use if in your code:

```javascript
		onSubmit(props) {
			this.props.createPost(props)
				.then(() => {
					browserHistory.push('/');
				});
		}
```

## combineReducers in Depth

Here is a simple sample of what the combineReducers function is doing:

```javascript
	var todos = (state = '', action) => {
	 console.log("todos");
	 return "todo";
	};
	
	var visFilter = (state = '', action) => {
	 console.log("visFilter");
	 return "visFilter";
	};
	
	var reducers = {
	 todos,
	 visFilter
	};
	
	const combineReducers = (reducers) => {
	 return (state = {}, action) => {
	 return Object.keys(reducers).reduce(
	 (nextState, key) => {
	 nextState[key] = reducers[key] (
	 state[key], action);
	 return nextState;
	 },{});
	 };
	};
	
	console.log(combineReducers(reducers)({}, 'DONE'));
```

Output will be:

```javascript
	"todos" //Getting logged when the todos reducer is run
	"visFilter" //Getting logged when the visFilter reducer is run
	//Final object created from line 27 above.
	[object Object] {
	 todos: "todo",
	 visFilter: "visFilter"
	}
```

First thing to remember is that the const combineReducers is returning a function that accepts an object containing the state pieces with their assigned reducers:

```javascript
//This is an example of what combineReducers function does. 
//Loops through the reducers and 
const combineReducers = (reducers) => {
	return (state = {}, action) => {
		return Object.keys(reducers)
            .reduce((nextState, key) => {
              //assigning the function calls
                nextState[key] = reducers[key] (state[key], action);
                return nextState;
            },{});
	};
};
```
Next, when executing the combineReducers function, you will get back another function, which will accept the state and action parameters. 
Note, this is what our reducers need. But at this point, the state is the full state. The next part is where we dole out the pieces of state to the correct reducers.

`	return Object.keys(reducers)`

Object.keys will return an array of the keys of the object passed. In this case it will return the pieces of our state object: 

` ["todos", "visFilter"]`

Next, we will take this array of keys and pass them to the reduce() function

```javascript
return Object.keys(reducers).reduce(
	(nextState, key) => {
		nextState[key] = reducers[key] (
		state[key], action);
		return nextState;
		},{});
```

The reduce function takes a callback function with 4 parameters (prevValue, CurrValue, CurrentIndex, Array). T
he reduce function takes two arguments, the callback function and an optional intial value. However, if the initial value is supplied, then it is used as the first parameter (not confusing at all!).

So in our above example, we are passing the initial value ({}), so the **nextState** argument is actually the **CurrValue** parameter and the **Key** is the **Index.** 

Here is an example of how the reduce function works on the array Object.keys(reducers) produces

```javascript
	var reducers = {
	 todos,
	 visFilter
	};
	
	Object.keys(reducers).reduce((nextState, key) => {
	 console.log(key);
	 console.log(nextState);
	 nextState[key] = 1;
	 console.log(nextState);
	 return nextState;
	 }, {});
```

I'm not returning any functions, but this allows you to see how reduce() works. It starts with the initial value (an empty object) and the first pass through reduce sends the Initial state as nextState (an empty object). The key will be the first "reducer" sent in the form of the object keys. In this example, the reducers object contains two key "todos" and "visFilter", so "todos" will be passed as the first key. 

When we take what is currently an empty object and do this: nextState[key], JavaScript will add that key if it doesn't exist or overwrite with the value we are setting it equal to.

Last (and very important) we MUST return the nextState. This is the whole idea of a reduce function, it builds something based on each iteration.

Ok, back to the actual combineReducers function. When you invoke combineReducers and pass it an argument (an object defining the reducing functions), it returns to you a function. E.g. the first return statement. Note that this returned function takes to arguments, the state and action, just what our reducers need!

```javascript
	const combineReducers = (reducers) => {
	 return (state = {}, action) => {
	 return Object.keys(reducers).reduce(
	 (nextState, key) => {
	 nextState[key] = reducers[key] (
	 state[key], action);
	 return nextState;
	 },{});
	 };
	};
```

So, when redux finally call the function that combineReducers creates, it passes the whole state tree and the action called. Then our Object.keys().reduce does the magic of actually calling the reducers with just their portion of the state tree and reducing it back into a single state tree.

Note that in line 5 above, the reducers[key](...). We are invoking the function in the reducers[key].

Example: 

```javascript
	var reducers = {
	 todos,
	 visFilter
	};
```

If todos and visFilter both reference functions, then the following would invoke the function.

` reducers["todos"]({}, "New Todo");`

## Observe for Changes in Store

This is a pattern I found that allows you to watch for changes in to the data in your store and run some function when a change is found.

[Observer Pattern](https://github.com/reactjs/redux/issues/303#issuecomment-125184409)

Bottom line, this allows us to check the current state with the next state and determine if we should update. 

 **The parameters:** 

 **store -** the redux store variable

 **select -** a function that pulls the piece of state we want to check

 **onChange -** a function that will be invoked if the current and next state are different

```javascript
function observeStore(store, select, onChange) {
	let currentStatetvShows, currentStateShowData;

	function handleChange() {
		let nextStateObj = select(store.getState());

		let nextStatetvShows = nextStateObj.tvShows;
		let nextStateShowData = nextStateObj.showData;
		if (nextStatetvShows !== currentStatetvShows || nextStateShowData !== currentStateShowData) {
			currentStatetvShows = nextStatetvShows;
			currentStateShowData = nextStateShowData;
			onChange(currentStatetvShows, currentStateShowData);
		}
	}

	let unsubscribe = store.subscribe(handleChange);
	handleChange();
	return unsubscribe;
}

var unsubscribe = observeStore(store, (state) => {
	return {tvShows: state.tvShows, showData: state.showData}}, 
	(tvShows, showData) => tvMaze.saveShowData(tvShows, showData)
);
```

On lines 21-23, we call the observeStore() function, which, I believe, sets up a closure around the handleChange() function, which is what the store "subscribes" to. 
Which means that whenever a change in state occurs, the handleChange function will be called.

Because this was initiated inside the original observeStore() call, it is a closure, so the handleChange() function has access to the store, select and onChange arguments as well as any variables created outside of it (within the observeStore() function). 

This closure is what allows us to retain the state the last time that handleChange was called. Closure is cool.

Also note that the second and third parameters sent to the observeStore() function are FUNCTIONS. The **select** function determines what state gets returned from the state object.

The **onChange** function does what we want done where there is a state change in the part of the state we are observing.

## Middleware

Middleware is

Some useful pieces of middleware are:

3.  [redux-logger ](https://github.com/evgenyrodionov/redux-logger) - Which is a logger/debugger for redux
4.  redux-promise
5.  redux-thunk



## Selectors

Selectors are not a feature of redux or external library, but a concept.

Selectors are just regular functions, but they can be used to modify state data.

Here is a great video overview:
[Redux Selectors](https://www.youtube.com/watch?v=frT3to2ACCw)

