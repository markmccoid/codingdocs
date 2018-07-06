# Redux-Actions Module

[Docs](https://github.com/redux-utilities/redux-actions)

`redux-actions` is a npm module designed to reduce some of the boilerplate of redux, while at the same time creating consistency by implementing the use *Flux Standard Actions* in your Action Creators.

## Flux Standard Action

The flux standard action shape is as follows:

```javascript
let standardActionShap = {
    type: TYPE_CONSTANT,
    payload: whateverYouWant,
    error: boolean,
    meta: additionalData
}
```

## createActions and createAction

These functions streamline the process of creating action creator functions. [docs](https://redux-actions.js.org/api-reference/createaction-s)

`createAction` returns a function that will return an action:

```javascript
export const increment = createAction('INCREMENT');
export const decrement = createAction('DECREMENT');

increment(); // { type: 'INCREMENT' }
decrement(); // { type: 'DECREMENT' }
increment(10); // { type: 'INCREMENT', payload: 10 }
decrement([1, 42]); // { type: 'DECREMENT', payload: [1, 42] }
```

Note, how the createAction will take anything that you pass as an argument and make it the payload.  However, it is only one item you can pass.  If you need multiple things, store them in an object and pass that to the function created with `createAction`.

If you want to do something with the payload before returning the action, you can also pass a *payload creator* function.

For example, if you wanted all text to be upper case, you could do the following:

```javascript
export const saveText = createAction('SAVE_TEXT', text => text.toUpperCase());

saveText('text'); // { type: SAVE_TEXT, payload: 'TEXT' }
```

Since you probably have multiple actions creators, you can speed all this up by using the **createActions** function.

`createActions` takes two arguments, the first being an ActionMap.  You could just pass that.  It maps each of your Actions types to a *payload creator* function.  If you don't want to do anything with the payload just use and identity function (x => x).

```javascript
createActions({
  ADD_TODO: x => x, // payload creator (identity function)
  INCREMENT: x => x,
  DECREMENT: x => x,
  SAVE_TEXT: text => text.toUpperCase() // payload creator
});
```

For those items that just need an identity function, i.e. you are not changing the payload.  You can simply pass those as separate parameters after the ActionMap.

```javascript
createActions({
  SAVE_TEXT: text => text.toUpperCase() // payload creator
}, 
  ADD_TODO,
  INCREMENT,
  DECREMENT
);
```





## Error Handling

When using `createAction`or `createActions`, if you pass an error object as the payload, the `error` property will automatically be set to true.  You can use this error flag in the reducer to check if the payload is an error object.

Note, you must send an actual error object to get this flag to be set.

```javascript
{
    type: TYPE_CONSTANT,
    payload: Error Object,
    error: true
}
```

## handleActions

`handleActions` is used to replace the standard switch statement in your reducer.

It will output a reducer that you can either export or use `combineReducers` on.

You pass the `handleActions` function an object with your Action Types as properties and the reducer functions as values to the Action Type property.  Also, you must send a second argument, which is the default state.

Lastly, notice on AUTH_LOGIN, here we want to differential between an error state and a normal state.  To do this, instead of a function for the value, we pass an object, with two keys of it's own.  **view** and **throw**.  **view** is the regular reducer and **throw** is reducer to run if the error property is true.

```javascript
const authReducer = handleActions({
	AUTH_LOGOUT: (state, action) => {
		return { uid: '', status: 'SUCCESS', message: '' };
	},
	AUTH_SET_STATUS: (state, action) => {
			return { ...state, status: action.payload, message: '' };
	},
	AUTH_LOGIN: {
		next: (state, action) => {
			return { uid: action.payload, status: AUTH_SUCCESS, message: '' };
		},
		throw: (state, action) => {
				return { uid: '', status: AUTH_ERROR, message: action.payload}
		}
	}
}, authDefault)
```

Using the `handleActions` function, you can also direct an error to a different path in the reducer