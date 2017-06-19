# Lodash Library

## Contents
- [range](#range)

## range

<a name="range"></a>

### _.range([start=0], end, [step=1])
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
## _.range([start=0], end, [step=1])
Returns an array of numbers.  

---

## transform

<a name="transform"></a>

`_.transform(object, [iteratee=_.identity], [accumulator])`

An alternative to _.reduce; this method transforms object to a new accumulator object which is the result of running each of its own enumerable string keyed properties thru iteratee, with each invocation potentially mutating the accumulator object. If accumulator is not provided, a new object with the same [[Prototype]] will be used. The iteratee is invoked with four arguments: (accumulator, value, key, object). Iteratee functions may exit iteration early by explicitly returning false.

**Arguments**

**object (Object):** The object to iterate over.

**[iteratee=_.identity] (Function):** The function invoked per iteration.

**[accumulator] (*):** The custom accumulator value.

**Example:**
```javascript
//Get the field information for all the groups for the passed appName.
//We are transforming the group field data from an array of fields with group ids,
//to an object with the group ids as object keys
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
