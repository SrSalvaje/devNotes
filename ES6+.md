# ES6

Reference for js patterns, snipets, methods (etc...) that I use often with an emphasis but not limited to ES6+.

## Table of Contents

* [var, let, const](#var-let-const)
* [Arrow Functions](#arrow-functions)
* [Default Function Arguments](#default-function-arguments)
* [Template Strings](#template-strings)
* [New String Methods](#new-string-methods)
* [Destructuring](#destructuring)
* [The `for of` loop](#the-for-of-loop)


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

**Nesting template literals:**
You can nest template literals inside template literals, very useful to loop over objects, use conditional statements or pass "render functions":

With `.map()`:
```js
  const markup = ` //first set of backticks
    <ul class="dogs">
      ${dogs.map(dog => `//second set
        <li>
          ${dog.name}
          is
          ${dog.age * 7}
        </li>`).join('')}
    </ul>
  `;
```
With a ternary operator:

```js
    const markup2 = `
    <div class="song">
      <p>
        ${song.name} — ${song.artist}
        ${song.featuring ? `(Featuring ${song.featuring})` : ''}
      </p>
    </div>
  `;
  ```

With a render function a la React:
```js
 const beer = {
    name: 'Belgian Wit',
    brewery: 'Steam Whistle Brewery',
    keywords: ['pale', 'cloudy', 'spiced', 'crisp']
  };

  function renderKeywords(keywords) {
    return `
      <ul>
        ${keywords.map(keyword => `<li>${keyword}</li>`).join('')}
      </ul>
    `;
  }

  const markup = `
    <div class="beer">
      <h2>${beer.name}</h2>
      <p class="brewery">${beer.brewery}</p>
      ${renderKeywords(beer.keywords)}
    </div>
  `;
```

### Tagged Template Literals

A tag is a function that process the template literal instead of the browser, it allows us to have more control over how the template will be converted to a string.

In the following example  we use a tagged template literal to add style to the string. The tagged function is a regular function, what changes is the way we invoke it:

```js
function highlight(strings, ...values) {
    let str = '';
    strings.forEach((string, i) => {
        //we are wrapping every value in the template string in span tags
      str += `${string} <span contenteditable class="hl">${values[i] || ''}</span>`; 
      //since there is one value less than strings, we check if value  is defined otherwise return empty string.
    });
    return str;
  }
  //by tagging the template we gain the ability to modify it before it gets parsed into a tring.
  //the value of sentence is going to be whatever highlight returns
  const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
  document.body.innerHTML = sentence;
  console.log(sentence);
```
Note the parameters of `highlight`, the first one corresponds to the strings of the template literal, the second one to the destructured variables (two in this case `name` and `age`).

The `strings` are broken into the largest possible chunks before a variable appears:
1. `My dog's name is`
1. `and he is`
1. `years old`

The array of strings will **always** be one element longer than the variables, even if we add a variable at the end:

```js
const sentence = highlight`My dog's name is ${name} and he is ${age} years old ${gender}`;
//sentence would be equal to:
/* My dog's name is <span class="hl">Snickers</span> and he is <span class="hl">100</span>years old <span class="hl"></span>
```
In this case, the last string, after `gender` would be an empty space `""`.

**Important!**: when dealing with user generated data you should **ALWAYS** sanitize the template literal!

### Sanitizing user data with template literals

Example by [Wes Bos](https://es6.io/)

```js

<script src="https://cdnjs.cloudflare.com/ajax/libs/dompurify/0.8.2/purify.min.js"></script>
<script>
  function sanitize(strings, ...values) {
    const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i] || ''}`, '');
    return DOMPurify.sanitize(dirty);
  }

  const first = 'Wes';
  const aboutMe = `I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

  const html = sanitize`
    <h3>${first}</h3>
    <p>${aboutMe}</p>
  `;

  const bio = document.querySelector('.bio');
  bio.innerHTML = html;
</script>
```

[home][home]

## New String Methods

ES6 adds 4 new methods:
1. [`startsWith()`](#startswith)
    * Takes 2 params, second one is optional. First is the string we want to check for, second one is an integer and represents the number of characters we want to skip before looking for a match. Returns a boolean, not case sensitive. 
1. [`endsWith()`](#endswith)
    * similar to `startsWith` but the second parameter stops the search after the given amount of characters.
1. [`includes()`](#includes)
    * checks if the string includes any given string.

1. [`repeat()`](#repeat)
    * takes a string and repeats by the given integer, helpful for leftPad functions ;) 
    
    ```js
    function leftPad(str, length = 20) {
    return `→ ${' '.repeat(length - str.length)}${str}`;
    }
    ```
## Destructuring

From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment):

>The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

### Destructuring objects

In its most basic usage, its a simpler way of unpacking values into variables: 

```diff
const person = {
    first: "Jon",
    Last: "Aresti",
    country: "Mexico",
    current: "Malmö"
}
-const first= person.first;
-const last = person.last;
+const {first, last} = person;
```

It begins to really shine with nested data, note how you can also assign custom names to the object properties:

Example from [Wes Bos](https://es6.io/):

```diff
const wes = {
    first: 'Wes',
    last: 'Bos',
    links: {
        social: {
        twitter: 'https://twitter.com/wesbos',
        facebook: 'https://facebook.com/wesbos.developer',
        },
        web: {
        blog: 'https://wesbos.com'
        }
    }
}

-const twitter = wes.links.social.twitter;
-const facebook =  wes.links.social.facebook;
+const { twitter: tweet, facebook: fb } = wes.links.social;
```
#### setting defaults with destructuring

In this example we create a default settings object and then destruture it to add extra values, behinds the scenes javascript looks for each property in the original object, if it exists, it assigns that value (regadless of the fact we passed another value) and if it doesnt, it creates the property.

```js
const settings = { width: 300, color: 'black' }  // height, fontSize
const { width = 100, height = 100, color = 'blue', fontSize = 25} = settings;
```
After being destructured `settings`will look like this:

```js
{
    width:300, //the value originally assigned
    height:100, //not present in the original object
    color: "black", //the value of the original property
    fontSize: 25 //not present in the original object
}
```
This is handy when passing setting objects to functions!

### Destructuring Arrays

Similar to objects but we use `[]`instead of `{}`. Furthermore, the destructuring is index based:

```js
  const details = ['Jon Miren', 0755555, 'mysite.com'];
  const [name, id, website] = details;
  console.log(name, id, website);// Jon Miren, 07555555, mysyte.com
```

If the array has more elements than the destructuring assignment, these are ignored:

```js
const details = ['Jon Miren', 0755555, 'mysite.com', "random", "values", "and", "stuff"];
const [name, id, website] = details;
//returns the same as the previous example
```
If you need the rest of the values, you can use the rest operator `...`

```js
const team = ['Wes', 'Harry', 'Sarah', 'Keegan', 'Riker'];
const [captain, assistant, ...players] = team;
//playes is assigned an array with the rest of the values
players = ['Sarah', 'Keegan', 'Riker']
```
### Swaping variables

You can swap the value of variables without the need of a temp variable (example by [Wes Bos](https://es6.io/)): 

```js
  let inRing = 'Hulk Hogan';
  let onSide = 'The Rock';

  console.log(inRing, onSide);//Hulk Hogan, The Rock
  [inRing, onSide] = [onSide, inRing];
  console.log(inRing, onSide);// The Rock, Hulk Hogan
```

### Destructuring functions, multiple returns and named defaults

You can return an object from a function and destructure multiple values from it (its as if you were **returning multiple values!**):

```js
function convertCurrency(amount) {
    const converted = {
        USD: amount * 0.76,
        GPB: amount * 0.53,
        AUD: amount * 1.01,
        MEX: amount * 13.30
    };
    return converted;
}
const {USD, GBP, AUD, MEX} = convertCurrency(100);
```
**Named Defaults**

If you pass declare the default values as a destructured object, you can then omit values during the callback as well as **not worry about the order of the parameters!!**
```js
function tipCalc({ total = 100, tip = 0.15, tax = 0.13 } = {}) {
    return total + (tip * total) + (tax * total);
}

const bill = tipCalc({ tip: 0.20, total: 200 });
console.log(bill);
```
[home][home]

## The `for of` loop

Can be used in any type of data that is iterable, (arrays, strings, sets, maps, generators...). The best way to appreciate `for of` is to understand were it stands in contrast to other loops.

`for` The syntax is  its main drawback; messy and "hard" to read.

```js
for (let i = 0; i < players.length; i++) {
    console.log(players[i]);
}
```

`forEach()` For each doesnt allow you to abort the iteration.

```js
players.forEach((player) => {
  console.log(player);
  if(player === 'Jon') {
   break;//not valid
 }
});
```

`for in` It will iterate trough **everything** in the array, including methods added to the prototype, which will bring complicatiosn with libraries that mess with the prototype.

```js
for (const index in cuts) {
   console.log(cuts[index]);
}
```

**`for of`**  Unlike `for in` it only iterates trough the array elements and it also allows you to stop the iteration with `break` as well as use `continue`.

```js
for (const cut of cuts) {
    if(cut === 'Brisket') {
      continue;
    }
    console.log(cut);
  }
```
The main drawback is that you cant use it in objects and that you can´t directly access the index, however, if you use the array interator `Array.entries()` then you get access to the index!

```js

const players = ['jon', 'sofia', 'leo', 'diego'];
for (const player of players.entries(){
    console.log(player)
});
// logs
//[0, 'jon']
//[1, 'sofia']
//[2, 'leo']
//[3, 'diego']

```

Furthermore, you can leverage destructuring!

```js

const players = ['jon', 'sofia', 'leo', 'diego'];
for (const [i, player] of players.entries(){
    console.log(`${player} is the ${i+1} team member`)
});
// logs
//'jon is the 1 team member' 
//' sofia is the 2 team member' 
//' leo is the 3 team member' 
//' diego is the 4 team member' 


```





[home][home]




[home]:#table-of-contents