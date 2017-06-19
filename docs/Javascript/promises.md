# Promises
Most important thing to understand about promises is how they are processed. Meaning, if you have a function called myPromise that returns a promise:

```javascript
	function test() {
	console.log("First Line in function");
		myPromise.then((data) => {
			console.log("Resolved Promise");
		});
		console.log("Line 4 in function");
	}
	//**************
	//--Output
	//First Line in function
	//Line 4 in function
	//Resolved Promise
```

The code will run to completion before the .then on a promise is even checked if it is resolved.