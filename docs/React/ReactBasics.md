# React Basics

## Contents

- [Functional and Class Components](#functional-and-class-components)
- [State](#state)
- [input basics](#input-basics)
- [env file](#env-file)

---

## Functional and Class Components

<div><a name="functional-and-class-components"></a></div>

Description of the difference between functional and class based components.

Sometimes functional are called "dumb" components and class based are "smart" or "container" components.  But in truth, the difference as I see it is that class based components can have state and functional cannot.  Functional components rely on the passed props to render.

---
## State

<div><a name="state"></a></div>

You can have internal state on Class components.  You can either set the initial state in a constructor OR if you don’t need a constructor you can simply set the state as a Class variable:

**State in Constructor**
```javascript
class Button extends React.Component {
	constructor(props) {
		super(props);
		this.state = { val1: 1 };
	}
	render() {
	 ...	
	}
}
```
**State as Class variable**
```javascript
class Button extends React.Component {
	state = { val1: 1 };
	
	render() {
	 ...	
	}
}
```
#### Setting State to a State Variable
If you need to use *setState()* to set a state variable to an existing state variable, use the function contract within the *setState()* function.  You can pass a function to *setState()* which will be passed the previous state as a parameter.

```javascript
this.setState((prevState) => {
	return ({ counter: prevState.counter + 1});
});
//OR with an implicit return 
this.setState( prevState => ({
	counter: prevState.counter + 1
}));
```
---

## Input basics 

<div><a name="input-basics"></a></div>

Usually a controlled input is best, but you can also use the react *ref* attribute on an input element.  The *ref* attribute takes a function and calls that function the form element is mounted.
This allows us to store a reference to that form element on the class.
```javascript
<input type="text"
	ref={(input) => this.userNameInput = input}
/>
<a onClick={() => console.log(this.userNameInput.value)} > Get Username</a>
```
## Env File

When using Firebase or other API that needs a API Key or other information that you do not want to put up to github, you can store this information in a *.env* file.

When using create-react-app, you can only access env variables using the format of **REACT_APP_...**

For Firebase, create a **.env** file in the root of your project with the following:

```
REACT_APP_FB_APIKEY=keydetails.......
REACT_APP_FB_AUTHDOMAIN=cartracker-.....firebaseapp.com
REACT_APP_FB_DATABASEURL=https://cartracker-871cc.firebaseio.com
REACT_APP_FB_PROJECTID=cartracker-871cc
REACT_APP_FB_STORAGEBUCKET=cartracker-871cc.appspot.com
REACT_APP_FB_MESSAGINGSENDERID=136622808562
```

Notice, no quotes!!

Next, in your Firebase.js file, insert this:

```javascript
const FB_API_KEY = process.env.REACT_APP_FB_APIKEY;
const FB_AUTHDOMAIN = process.env.REACT_APP_FB_AUTHDOMAIN;
const FB_DATABASEURL = process.env.REACT_APP_FB_DATABASEURL;
const FB_PROJECTID = process.env.REACT_APP_FB_PROJECTID;
const FB_STORAGEBUCKET = process.env.REACT_APP_FB_STORAGEBUCKET;
const FB_MESSAGINGSENDERID = process.env.REACT_APP_FB_MESSAGINGSENDERID;

const config = {
  apiKey: FB_API_KEY,
  authDomain: FB_AUTHDOMAIN,
  databaseURL: FB_DATABASEURL,
  projectId: FB_PROJECTID,
  storageBucket: FB_STORAGEBUCKET,
  messagingSenderId: FB_MESSAGINGSENDERID
};
```

DON'T FORGET to put your .env file in your .gitignore file!