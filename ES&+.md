# ES6

Reference for all the ES6 features and patterns I come across.

## Table of Contents
* [var, let, const](#var-let-const)
* [Arrow Functions](#arrow-functions)


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
 Arrow functions are written with fat arrows `=>` and the exact syntax depends on what you and how you are using it.
  1. If you have only one parameter (or none) and are using implicit return you write it like this: ` param => /*code*/;`.
  1. More than one parameter + implicit return: `(param1, param2) => /*some code*/;`.
  1. Without implicit return 
  ```js
   => {
      /*some code*/
      return /*what you want to return*/
  };
  ```
**Note:** The implicit return doesn't mean you have to write it as a one liner, you can sue `()` to spread the code over multiple lines, this is helpfull when using it in combination with array methods that expect a return (like `Array.map()`)

```js
=>(
    //code
    //more code
    //even more code
);
```


[home][home]

[home]:#table-of-contents