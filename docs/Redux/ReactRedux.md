# React-Redux

##NPM modules needed:

**Redux module** 

`npm install redux —save-dev`

**react-redux module** 

`npm install react-redux —save-dev`

This is the library needed to integrate redux into react.

---

Sample application structure:

![](https://s3-us-west-2.amazonaws.com/notion-static/nVcfflYJSw3SaJXVkMEJ_Untitled)

##Store via the Provider component

The Main app will exist in the **&lt;Provider&gt;** component. This allows the redux **store** to be made available to all app components/child components.

So, in the main file of your app, where you are running ReactDOM.render(), you will wrap all your components that need access to the store data in the Provider component.

The *store.subscribe()* function allows you to create a function that runs every time that state is changed.

This would be the place to save items to a DB.

```javascript
const {Provider} = require('react-redux');
var TodoApp = require('TodoApp');

var actions = require('actions');
var store = require('configureStore').configure();

store.subscribe(() => {
	var state = store.getState();
	console.log('new state', state);
	TodoAPI.setTodos(state.todos);
});

ReactDOM.render(
	<Provider store={store}>
		<TodoApp />
	</Provider>,
	document.getElementById('app')
);
```

## Injecting State into a Component

You will use the "*connect()*" function to inject the **store's** state into a component. It is done when you export the module:

```javascript
...
var {connect} = require('react-redux');
...

module.exports = connect(
	(state) => {
		return {
			todos: state.todos
		}
	}
	)(TodoList);
```

The return object tells the _**connect **_ function which state to assign to the props.

If you want all of the store's state to be pushed into the components props, then simply return state:

	...	var {connect} = require('react-redux');
	...
	
	module.exports = connect(
	(state) => {
		return state;
	})(TodoList);

If you don't need to get state in a component, but need to use the _**dispatch **_ function, then you can use the _**connect **_ function as follows:

	...
	var {connect} = require('react-redux');
	...
	module.exports = connect()(Todo);	

## Actions

[Redux Actions](http://redux.js.org/docs/basics/Actions.html)

Usually you will create a separate folder\files for your actions.

Redux actions tell redux reducers what action to perform on the **store.** Most likely this will be saving or modifying data in the store.

An action is an object, with a type property and other properties (the payload) that will be used by the reducer to complete the action.

Actions will be dispatched with the dispatch() function. You can directly pass an object to the dispatch function, but creating action generators seems to be the cleaner way to go:

```javascript
	export var setSearchText = (searchText) => {
		return {
			type: 'SET_SEARCH_TEXT',
			searchText: searchText
		}
	};
	
	export var addTodo = (text) => {
		return {
			type: 'ADD_TODO',
			text: text
		}
	};
	
	export var toggleShowCompleted = () => {
		return {
			type: 'TOGGLE_SHOW_COMPLETED'
		}
	};
	
	export var toggleTodo = (id) => {
		return {
			type: 'TOGGLE_TODO',
			id: id
		}
	};
```

## Reducers

[Redux Reducers](http://redux.js.org/docs/basics/Reducers.html)

Usually you will create a separate folder\files for your reducers.

Reducers take the **_action_** that is sent via the **_dispatch()_** function and modify the state of the store. However, reducers are [pure functions](https://www.notion.so/React-Redux-4f9cafe4bdce49229d019a12325319d4#11a76a10563643c89e88bb6c021258da) , which means they do not mutate any of the state information, but instead send back a new object/array/data.

You can use the spread operator (...) in ES6 to help with Arrays and Objects:

```javascript
	export var todosReducer = (state = [], action) => {
		switch (action.type) {
			case 'ADD_TODO':
				return [
					...state,
					{
						id: uuid(),
						text: action.text,
						completed: false,
						createdAt: moment().unix(),
						completedAt: undefined
					}
				];
			case 'TOGGLE_TODO':
				return [...state].map((todo) => {
					if (todo.id === action.id) {
						return Object.assign({}, todo, {
							completed: !todo.completed,
							completedAt: !todo.completed ? moment().unix() : undefined
						});
					} else {
						return todo;
					}
				});
			default:
					return state;
		}
```

You can see that [...arrayName] will return a new array with all of the items in arrayName. 

You can also do this with objects **{...objectName}** or you could use the **[Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)** function.

## Dealing with Pure Functions

The main thing with pure functions is that you can't change anything directly. So, if you want to add and item to an array, or modify a property on an object, you can't just push() to the array or do [obj.property](http://obj.property) = "some text", you must create a new object.

This can easily be accomplished using a few tools.

## Testing tools for pure functions

Testing this can be accomplished with the deep-freeze-strict module.