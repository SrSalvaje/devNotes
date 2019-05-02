# ES6

Reference for js patterns, snipets, methods (etc...) that I use often with an emphasis but not limited to ES6+.

## Table of Contents

* [var, let, const](#var-let-const)
* [Arrow Functions](#arrow-functions)
* [Default Function Arguments](#default-function-arguments)
* [Template Strings](#template-strings)


## var, let, const

The first difference is that Unlike `var`, you cant declare the same variable twice with `let` and `const`.

This works: 

```js

var x = 100;
/*some code*/

var x = 200;
```

Whereas this throws an error:

```js
let x = 100;

/*stuff*/
let x = 1000;
```
Secondly, you can update `let`´s value but not `const`´s. This **doesn't** mean `const`is immutable! You can still update the properties or elements of an array/object assigned to a `const`. 

### function vs block scope

`var`has function scope so in this case:

```js
var age = 100;

if(age > 12){
    var dogYears = age * 7;
    console.log(`You are ${dogYears} dog years old!`);
}
```

`dogYears` would have global global scope even though it was declared in between `{}`.

`let` and `const` on the other hand have block scope so in this case:

```js
if(age > 12){
    let dogYear = age * 7; 
    console.log(`You are ${dogYears} dog years old!`);
}
```

`dogYears` would not be available outside of the `if` statement. You can even declare a block to force scope:

```js
{
    const name = "Jon";
}
```


### `let` vs `const` vs `var`
The community has gathered around two views:
1. The **`var` is dead** view
    * Use `const` by default and `let` if rebinding is needed.
1. The **Kyle Simpson** approach
    1. Use `var` for top-level variables shared across many scopes.
    1. use `let`for localized variables in smaller scoped.
    1. Refactor `let` to `const` only after some code has been written and you are reasonably sure you have a case were there shouldn't be variable reassignment.

[home][home]

## Arrow Functions
 Arrow functions are written with fat arrows `=>` and the exact syntax depends on what you and how you are using it. When using arrow functions the value of `this` is inherited  from the parent scope of where its called from.
  1. If you have only one parameter (or none) and are using implicit return you write it like this: ` param => /*code*/;`.
  1. More than one parameter + implicit return: `(param1, param2) => /*some code*/;`.
  1. Without implicit return 
  ```js
   => {
      /*some code*/
      return /*what you want to return*/
  };
  ```
They are always anonymous functions **but** you can store them in a variable. However, they will still be anonymous in the stack trace.
```js
const myArrowfunction = ()=>{
    /*stuff*/
}
```
**Note:** The implicit return doesn't mean you have to write it as a one liner, you can sue `()` to spread the code over multiple lines, this is helpfull when using it in combination with array methods that expect a return (like `Array.map()`) or when you want to return an object.

```js
=>(
    //code
    //more code
    //even more code
);
```

### When **NOT** to use arrow functions

When you really need `this`:

```js
  const button = document.querySelector('#pushy');
  button.addEventListener('click', function() {
    console.log(this);
    this.classList.toggle('on');
  });
```

If you were to use an arrow function inside the `eventListener` `this`would be bind to the `window`because:
 >When using arrow functions the value of `this` is inherited  from the parent scope of where its called from.

When you need a method to bind to an object

```js  
const person = {
    points: 23,
    score() { //if you used an arrow function here `this`would once again be bound to the window
        console.log(this);
        this.points++;
    }
}
```

When you need to add a prototype method

```js
class Car {
    constructor(make, colour) {
        this.make = make;
        this.colour = colour;
    }
}

const beemer = new Car('bmw', 'blue');
const subie = new Car('Subaru', 'white');

Car.prototype.summarize = function() {
    return `This car is a ${this.make} in the colour ${this.colour}`;
};

```

When you need `arguments` object because you dont have access to the `arguments` object when using arrow functions

```js
const orderChildren = function() {
    const children = Array.from(arguments);
    return children.map((child, i) => {
        return `${child} was child #${i + 1}`;
    })
    console.log(arguments);
}
```

[home][home]

## Default Function Arguments

Before default function arguments we would add conditionals to the body of a function to check for undefined values.

```js
function calculateTripCost(distance, kmXLiter, gasPrice){
    milesXLiter = kmXLitter || 10; //if kmXLiter is undefined assign 10
    gasPrice = gasPrice || 2; //if kmXLiter is undefined assign 2

    let cost = (distance/milesXLitter)*gasPrice;
    console.log(cost);
}

calculateTripCost(20);
//logs 4
```

Using default arguments we can assign the defualt value directly:

```js

function calculateTripCost(distance, kmXLiter=10, gasPrice=2){
    let cost = (distance/milesXLitter)*gasPrice;
    console.log(cost);
}

calculateTripCost(20);
//logs 4
calculateTripCost(20, 2, 5);
//logs 50
calculateTripCost(20, undefined, 3);
//logs 6
```

[home][home]

## Template Strings

[home][home]



[home]:#table-of-contents