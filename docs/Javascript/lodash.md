# Lodash Library

[TOC]



## range - creates a range of numbers

**_.range([start=0], end, [step=1])**

Returns an array of numbers.  

- _.range(4);
***// => [0, 1, 2, 3]***
- _.range(-4);
***// => [0, -1, -2, -3]***
- _.range(1, 5);
***// => [1, 2, 3, 4]***
- _.range(0, 20, 5);
***// => [0, 5, 10, 15]***
- _.range(0, -4, -1);
***// => [0, -1, -2, -3]***
- _.range(1, 4, 0);
***// => [1, 1, 1]***
- _.range(0);
***// => []***
**_.range([start=0], end, [step=1])**

Returns an array of numbers.  

---

## transform - an alternative to the reduce function

`_.transform(object, [iteratee=_.identity], [accumulator])`

An alternative to _.reduce; this method transforms object to a new accumulator object which is the result of running each of its own enumerable string keyed properties thru iteratee, with each invocation potentially mutating the accumulator object. If accumulator is not provided, a new object with the same [[Prototype]] will be used. The iteratee is invoked with four arguments: (accumulator, value, key, object). Iteratee functions may exit iteration early by explicitly returning false.

**Arguments**

**object (Object):** The object to iterate over.

**[iteratee=_.identity] (Function):** The function invoked per iteration.

**[accumulator] (*):** The custom accumulator value.

**Example:**
```javascript
// Get the field information for all the groups for the passed appName.
// We are transforming the group field data from an array of fields with group ids,
// to an object with the group ids as object keys
/*
***data before******************:
	[{
		id: 11-11--111,
		fields: [{...}],
		...
	},
	{
		id: 22-22--222,
		fields: [{...}],
		....
	}
	]
	***data after******************:
	{
		11-11--111: [
			{fieldinfo...}
		],
		22-22--222: [
			{fieldinfo...}
		]
	}
*/
export const getGroupFieldData = appName => {
	return getGroups(appName)
		.then(groupData => {
			return _.transform(groupData, (result, value, key) => {
				value.fields.forEach(field => {
					(result[value.id] || (result[value.id] = [])).push(field);
				});
			}, {});
		});
}
```

---

## [_.keyBy](https://lodash.com/docs/4.17.4#keyBy) - Takes an array and create an object of objects with a key pulled from the array.

This function would be useful if you had an array of objects and you wanted to convert it to an object of objects:

```javascript
let array = [{id: 1, name: "mark", age: 21}, {id: 2, name: "john", age: 25}];
let newObj = _.keyBy(array, 'id');
// Result:
// {
//   1: {
//     age: 21,
//     id: 1,
//     name: "mark"
//   },
//   2: {
//     age: 25,
//     id: 2,
//     name: "john"
//   }
// }
```

----

## [_.map](https://lodash.com/docs/4.17.4#map) - Converts a collection into an Array (Obj of Objects -> Array)

This is useful if you store your data in an object with their Id as the keys.  It will allow you to convert this Object of Objects into an array.

map(collection, iteratee function)

Iteratee function takes (value, index/key, collection).

With this in mind, to get back an array of objects from an object of objects:

```javascript
let objOfObj = {
  1: {
    age: 21,
    id: 1,
    name: "mark"
  },
  2: {
    age: 25,
    id: 2,
    name: "john"
  }
};

let newArray = _.map(objOfObj, (val) => val);

console.log(newArray)
// [{
//  age: 21,
//  id: 1,
//  name: "mark"
// }, {
//  age: 25,
//  id: 2,
//  name: "john"
// }]

//If for some reason, the Keys of the original object were not in the inner objects and you needed them.  You could write your function as follows:
let newArray = _.map(objOfObj, (val, key) => {return {...val, key }}); //parseInt(key) if needed.

```

