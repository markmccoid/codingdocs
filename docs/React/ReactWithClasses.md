# React with Classes

A few idiosyncrasies with React when using the class structure that need to be watched out for.
1.  You don't always need a class, but can sometimes just create a functional component that returns JSX.
2. If you do create a class and have a constructor, then call super(props) in the constructor to get access to your props.
1. You only need a constructor if you are going to do something with props in the constructor.  Otherwise you can set state as a class static variable.
3. Initial state will be set in the construction by setting this.state = . Only place where mutating this.state is acceptable.
4. Any helper functions that are not part of the React lifecycle will need to be bound to _this_ , either through a bind in the constructor or using fat arrow functions when creating functions.

## Example class declaration:
```js
class GroupMain extends React.Component {
	constructor(props) {
		super(props);
		this.state = {
			page: ''
		};
}
	
	render() {
		return <div>Test Component</div>
	}
}
export default GroupMain;
```
**Binding Helper Functions** :

```js
class GroupMain extends React.Component {
constructor(props) {
	super(props);
	this.state = { page: '' };

	this.checkGroupExists = this.checkGroupExists.bind(this);
}
//Must be bound in constructor
checkGroupExists(newName, newDescription) {
	console.log(newName);
	...
}
//ES6 format automatically binds this for us
showAddGroup = () => {
	this.setState({page: "ADD"});
}

render() {
		...
	}
}
```

You can see that we are declaring the **checkGroupExists()** function as usual. When you do this, you will need to add a statement to the constructor function that binds this to the function:

```js 
this.checkGroupExists = this.checkGroupExists.bind(this);
```

However, if you declare your function using ES6 arrow notation, you will automatically bind this to the function.

```js
showAddGroup = () => {
	this.setState({page: "ADD"});
}
```

## Using PropTypes and Default Props
**Declaring Prop Type and Default Props**
With functions and ES6 classes, propTypes and defaultProps are defined as properties on the components themselves:
```js
	class Greeting extends React.Component {
	  // ...
	}
	
	Greeting.propTypes = {
	  name: React.PropTypes.string
	};
	
	Greeting.defaultProps = {
	  name: 'Mary'
	};	
```

You can also use a **static class property** to declare your default props:

```javascript
class Greeting extends React.Component {
	static defaultProps = { initialThingy: 0 };
}
```
## Default Props on a Stateless component (functional component)
With a stateless component you will destructure the prop property and give it a default.
```javascript
const Greeting = ({ intialThingy = 0 }) => {

}
```

**Examples of the different PropType validators**

```js
MyComponent.propTypes = {
	// You can declare that a prop is a specific JS primitive. By default, these
	// are all optional.
	optionalArray: React.PropTypes.array,
	optionalBool: React.PropTypes.bool,
	optionalFunc: React.PropTypes.func,
	optionalNumber: React.PropTypes.number,
	optionalObject: React.PropTypes.object,
	optionalString: React.PropTypes.string,
	optionalSymbol: React.PropTypes.symbol,

	// Anything that can be rendered: numbers, strings, elements or an array
	// (or fragment) containing these types.
	optionalNode: React.PropTypes.node,

	// A React element.
	optionalElement: React.PropTypes.element,

	// You can also declare that a prop is an instance of a class. This uses
	// JS's instanceof operator.
	optionalMessage: React.PropTypes.instanceOf(Message),

	// You can ensure that your prop is limited to specific values by treating
	// it as an enum.
	optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),

	// An object that could be one of many types
	optionalUnion: React.PropTypes.oneOfType([
		React.PropTypes.string,
		React.PropTypes.number,
		React.PropTypes.instanceOf(Message)
	]),

	// An array of a certain type
	optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),

	// An object with property values of a certain type
	optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),

	// An object taking on a particular shape
	optionalObjectWithShape: React.PropTypes.shape({
		color: React.PropTypes.string,
		fontSize: React.PropTypes.number
	}),

	// You can chain any of the above with `isRequired` to make sure a warning
	// is shown if the prop isn't provided.
	requiredFunc: React.PropTypes.func.isRequired,

	// A value of any data type
	requiredAny: React.PropTypes.any.isRequired,

	// You can also specify a custom validator. It should return an Error
	// object if the validation fails. Don't `console.warn` or throw, as this
	// won't work inside `oneOfType`.
	customProp: function(props, propName, componentName) {
		if (!/matchme/.test(props[propName])) {
			return new Error(
				'Invalid prop `' + propName + '` supplied to' +
				' `' + componentName + '`. Validation failed.'
			);
		}
	},

	// You can also supply a custom validator to `arrayOf` and `objectOf`.
	// It should return an Error object if the validation fails. The validator
	// will be called for each key in the array or object. The first two
	// arguments of the validator are the array or object itself, and the
	// current item's key.
	customArrayProp: React.PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
		if (!/matchme/.test(propValue[key])) {
			return new Error(
				'Invalid prop `' + propFullName + '` supplied to' +
				' `' + componentName + '`. Validation failed.'
			);
		}
	})
};
```