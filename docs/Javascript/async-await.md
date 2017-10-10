# Async/Await feature of ES6

You must understand that the async/await feature is built on top of promises.

To give an example of using async/await, we will first need to have a function that returns a promise to work with:

```javascript
//Simple function that returns a promise and will resolve with a message in 'amount' of time.
function breathe(amount) {
  return new Promise((resolve, reject) => {
    if (amount < 500) {
      reject('That is too small of a value');
    }
    setTimeout(() => resolve(`Done for ${amount} ms`), amount);
  });
}
```

Next, we need to create an *async* function to work with the promise:

```javascript
//an async function must begin with keyword 'async'
async function go() {
  console.log(`Starting to breathe`);
  const res = await breathe(1000);
  console.log(res);
  const res2 = await breathe(500);
  console.log(res2);
  const res3 = await breathe(750);
  console.log(res3);
  const res4 = await breathe(900);
  console.log(res4);
  console.log('end');
}
```

You must begin a function that is going to use *await* with the keyword *async*.

Keep in mind that this is essentially creating a function that will be **synchronous**, but only when the code is running inside of it.

```javascript
//Using the breathe function and the async function go
console.log('start');
go();
console.log('end');
//result 
//start
//end
//Starting to breathe
//1000
//500
//750
//900
```

A few key thing to note:

1. Every *async* function will return a **promise**
2. Most things you *await* will be a **promise**
3. *await* can only be used inside an *async* function
4. *async* makes that function, in essence, synchronous

## Error Handling

There are a couple of ways to handle errors, but on the surface, the **async/await** is creating synchronous code, so we can just use a *try/catch* block.

```javascript
async function go() {
  try {
    const res = await breathe(1000);
    console.log(res);
    const res2 = await breathe(500);
    console.log(res2);
    const res3 = await breathe(750);
    console.log(res3);
  } catch(err) {
    console.err(err);
  }  
  
```

OR if you don't use a try/catch in your async function, you could put a catch on the calling function:

```javascript
async function go() {
  const res = await breathe(1000);
  console.log(res);
  const res2 = await breathe(500);
  console.log(res2);
  const res3 = await breathe(750);
  console.log(res3);
}

go().catch((err) => console.log(err));
```

However, in my testing I have found that if you have a **try/catch** in the async function and a **.catch** calling function and there is an error, the **try/catch** is the one that will be used.

## .then() on the async function call

You can use a .then() on the call to the async function.  The .then will be run after the function completes and will be passed whatever is returned from the function.

```javascript
async function go() {
  console.log(`Starting to breathe`);
  const res = await breathe(1000);
  console.log(res);
  const res2 = await breathe(500);
  console.log(res2);
  const res3 = await breathe(750);
  console.log(res3);
  const res4 = await breathe(900);
  console.log(res4);
  console.log('end');
  return ('End of Async GO');
}

//async go will complete and when complete the .then() will run
go().then(val => console.log(`outside then called with - ${val}`));
```

## Higher Order Function to Catch Repetitive Errors

Many times you will want specific error handling for your async functions, however, there will be times when the same errors will need to be handled in multiple async functions.  

Instead of duplicating this error handing code over and over again, it is best to use a higher order function to capture and deal with the errors.

[Wes Box example in his ES6 Course](https://courses.wesbos.com/account/access/57a1679fa9a636e308fdfb2f/view/235537238)

The below example also adds parameters that are passed to the go function.  This is just to show how these would be handled in the higher order function.

```javascript
function breathe(amount) {
  return new Promise((resolve, reject) => {
    if (amount < 500) {
      reject('That is too small of a value');
    }
    setTimeout(() => resolve(`Done for ${amount} ms`), amount);
  });
}
//Our Higher Order function
function catchErrors(fn) {
  return function (...args) {
    return fn(...args).catch((err) => {
      console.error('Ohhhh nooo!!!!!');
      console.error(err);
    });
  }
}
async function go(name, last) {
  console.log(`Starting for ${name} ${last}!`);
  const res = await breathe(1000);
  console.log(res);
  const res2 = await breathe(500);
  console.log(res2);
  const res3 = await breathe(750);
  console.log(res3);
  const res4 = await breathe(900);
  console.log(res4);
  console.log('end');
}

const wrappedFunction = catchErrors(go);
wrappedFunction('Wes', 'Bos');
```

Note that the catchErrors function accepts the function (fn) and then returns a function.  The function it returns uses the ***...args*** (spread operator on the args) to accept any number of arguments that may be passed into the async function.  

When this higher order function (which has wrapped our async function) finally gets called, we pass the arguments to our async function **fn(...args)** and tack on a **.catch()** to handle any errors.  

We do return the fn() call, which return a promise that may and may not be used.

