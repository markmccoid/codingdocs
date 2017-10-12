## Ramda
(Ramda Homepage)[http://ramdajs.com/]
(Ramda Documentation Page)[http://ramdajs.com/docs/]

### Why Ramda?

There are already several excellent libraries with a functional flavor. Typically, they are meant to be general-purpose toolkits, suitable for working in multiple paradigms. Ramda has a more focused goal. We wanted a library designed specifically for a functional programming style, one that makes it easy to create functional pipelines, one that never mutates user data.

## Useful Ramda Functions
### [R.pick([array of string keys], Object To copy from) -> returns new Object](http://ramdajs.com/docs/#pick)
Returns a partial copy of an object containing only the keys specified. If the key does not exist, the property is ignored.
```javascript
let obj = {
  password: 'x',
  name: 'mark',
  lastName: 'mccoid',
  box: 1
};
console.log(R.pick(['lastName', 'name'], obj));
//returns:
//[object Object] {
//  lastName: "mccoid",
//  name: "mark"
//} 
```

### [R.omit([array of string keys], Object To copy from) -> returns new Object](http://ramdajs.com/docs/#omit)
Opposite of R.pick().  Returns a partial copy of an object omitting the keys specified.
```javascript
R.omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, c: 3}
```

### [R.path([path], Object To Look into)](http://ramdajs.com/docs/#path)
Retrieve the value at a given path.  This is useful, because instead of throwing an error if the path doesn't exist,  you get _undefined_ returned.
Think of trying to get an Object property value that is nested, but that property doesn't have to exist.  Path will just returned _undefined_.  However, take a look at R.pathOr(), which will actually give you a default value back.
```javascript
R.path(['a', 'b'], {a: {b: 2}}); //=> 2
R.path(['a', 'b'], {c: {b: 2}}); //=> undefined
```

### [R.pathOr(Default, [path], Object To Look into)](http://ramdajs.com/docs/#pathOr)
If the given, non-null object has a value at the given path, returns the value at that path. Otherwise returns the provided default value.
```javascript
R.pathOr('N/A', ['a', 'b'], {a: {b: 2}}); //=> 2
R.pathOr('N/A', ['a', 'b'], {c: {b: 2}}); //=> "N/A"
```

### [R.prop(Property To look for, Object To look in)](http://ramdajs.com/docs/#prop)
Returns a function that when supplied an object returns the indicated property of that object, if it exists.  NOTE: Since this returns a function, it can be curried.
```javascript
R.prop('x', {x: 100}); //=> 100
R.prop('x', {}); //=> undefined
//Or Curry style
let checkForx = R.prop('x');
console.log(checkForx({x: 100})) //=> 100
```

[### R.project([Props to Select], Array of Objects)](http://ramdajs.com/docs/#project)
Reasonable analog to SQL select statement.
```javascript
var abby = {name: 'Abby', age: 7, hair: 'blond', grade: 2};
var fred = {name: 'Fred', age: 12, hair: 'brown', grade: 7};
var kids = [abby, fred];
R.project(['name', 'grade'], kids); //=> [{name: 'Abby', grade: 2}, {name: 'Fred', grade: 7}]
```