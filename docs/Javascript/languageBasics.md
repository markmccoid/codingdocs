## && and || Logical Operators

Two important things to know about these operators:
    1. They short-circuit evaluations
    2. They evaluate to their last evaluated operator

**Short-circuiting Evaluations**
For **&&** This means that if the first operand evaluates to false, the second operand is never evaluated.  This can be used to hide and show values based on an operand that evaluates to a boolean:
```javascript
var showMe = false;
//The "do something" code will only be evaluated if showMe is true.
showMe && //do something;
```

For **||** this means that if the result of the first operand is **true**, the second operand will not be evaluated.
This is useful when you want to return a default value for a property that doesn't exist on an object:
```javascript
let obj = {job: 'work'};
obj.name || 'default'
//returns 'default'
```
You can use these together to return a default value for a object that doesn't exist
```javascript
//if obj is undefined, 'mark' is returned.
obj && obj.name || 'mark'
//most useful if you are looking for a sub property that might not exist
obj = {name: 'mark', images: {small: 'url'}}
// now we are looking for a large image:
obj.images && obj.images.large || 'large default'
```
Note that if you use the && and || outside of an if, you will not get a boolean returned (unless boolean types were used), instead you get the expression returned.

If you want to get the actual boolean returned, then you can use the logical not (!), but you will have to use **!!**.  If you just use on !, you will get the opposite or the "not" value.  Using two !!, returns the actual boolean. So 
```javascript
!true
//returns false
!!true
//returns true
!!('cat' && 'dog')
//returns true
```
## Destructuring 

Allows you to pull variables out of object easily:

```javascript
const person = {
  name: 'mark',
  age: 19,
  address: {
    street: '5068 ...',
    city: 'wherever',
    state: 'confusion'
  } 
};

let { name, age } = person;
//If you want to rename:
let { name: firstName, age } = person;
//If you want to set a default value
let { name, age = 18 } = person;
//If you want to rename and set a default value
let { name, age: drinkingAge = 18 } = person;
```

