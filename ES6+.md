# ES6

Reference for js patterns, snipets, methods (etc...) that I use often with an emphasis but not limited to ES6+.

## Table of Contents

|Es6+|ES6+|
|:-----------|:--------------|
|[var, let, const](#var-let-const)|[Arrow Functions](#arrow-functions)|
|[Default Function Arguments](#default-function-arguments)|[Template Strings](#template-strings)|
|[New String Methods](#new-string-methods)|[Destructuring](#destructuring)|
|[The `for of` loop](#the-for-of-loop)|[New Array Methods](#new-array-methods)|
|[`...spread` and `...rest`](#spread-and-rest)|[Object Literal Upgrades](#object-literal-upgrades)|
|[Promises](#promises)|[Symbols](#symbols)|
|[Modules](#modules)|[Classes](#classes)|
|[Generators](#generators)|[Proxies](#proxies)|
|[Sets and WeakSets](#sets-and-weaksets)|[Map and WeakMap](#map-and-weakmap)|
|[Async+ Await Flow Control](#async-+-await-flow-control)|[ES7 and Beyond](#es7-and-beyond)|
|||
|||

 [](#)




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

Another advantage is that it allows us to iterate trough array like objects such as `arguments` or DOM collections.

[home][home]


## Iterating over objects

Part of ES2017 is `Object.entries()` and `Object.values()` both them have good browser support in 2019 and can also be polyfilled. 

### `.entries()`
From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries):

> Returns an array of a given object's own enumerable ***string-keyed property*** [key, value] pairs, in the same order as that provided by a for...in loop (the difference being that a `for-in` loop enumerates properties in the prototype chain as well). The order of the array returned by `Object.entries()` does not depend on how an object is defined. If there is a need for certain ordering then the array should be sorted first like 

```js
Object.entries(obj).sort((a, b) => b[0].localeCompare(a[0]));
```
To iterate over it you can mix it with `for of`

```js
const players = { first: 'Jon', second: 'Leo', third: 'Alex' };

for(const property of Object.entries(players)){
    console.log(property)
}

// ["first", "Jon"]
// ["second", "Leo"]
// ["third", "Alex"]
```

### `.values()`

[From MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Object/values)

>Returns an array of a given object's own enumerable property values, in the same order as that provided by a for...in loop (the difference being that a >for-in loop enumerates properties in the prototype chain as well).

Its essentially the opposite of `Object.keys()`.

```js
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.values(object1));
// expected output: Array ["somestring", 42, false]
```
### `for ... in`

The `for...in` statement iterates over all non-Symbol, enumerable properties of an object.

```js
const apple = {
    color: 'Red',
    size: 'Medium',
    weight: 50,
    sugar: 10
  };

  for (const prop in apple) {
    const value = apple[prop];
    console.log(value, prop);
  }
// Red color
// VM83:10 Medium size
// VM83:10 50 "weight"
// VM83:10 10 "sugar"
```

[home][home] 

## New Array Methods

**`Array.from()`** 

Takes something arrayish and turns it into an array, it also takes an optional function that will be applied to every array element.

```js
const people = document.querySelectorAll('.people p');
const peopleArray = Array.from(people, person => {
    console.log(person);
     return person.textContent;
});
```

**`Array.of()`**

Creates an array from the arguments you pass.

```js
const newArray = Array.of(1,2,3,4,5,6,7,8)
```

**`.find()`**

Returns the value of the first element in an array that passes a tests, which we provide as a function.

```js
const code = 'VBgtGQcSf';
const post = posts.find(post => post.code === code);
```

**`.findIndex()`**

Similar to .find() but returns the index of the first element to that satisfies the conditions of the test.


**`.some()`** and **`every()`**

Not part of ES6 but quite helpful, they return true or false based on wether some or every item in the array meets the condition.

```js
const ages = [32, 15, 19, 12];

// 👵👨 is there at least one adult in the group?
const adultPresent = ages.some(age => age >= 18);
console.log(adultPresent);

// 🍻 is everyone old enough to drink?
const allOldEnough = ages.every(age => age >= 19);
console.log(allOldEnough);
```
[home][home] 

## `...spread` and `...rest`

Takes all items from an iterable and apply it to the containing array.

```js
const myName = "Jon";
const speadMyName = [...myName]
//["J", "o", "n"]

const featured = ['Deep Dish', 'Pepperoni', 'Hawaiian'];
const specialty = ['Meatzza', 'Spicy Mama', 'Margherita'];
const pizzas = [...featured, ...specialty];

//["Deep Dish", "Pepperoni", "Hawaiian", "Meatzza", "Spicy Mama", "Margherita"]
```
You can also use it to copy an array instead of `.concat()`

```diff
-const fridayPizzas = [].concat(pizzas);
+const fridayPizzas = [...pizzas];
```

Or to create an array from an array like object (like a DOM node):

```diff
-const people = Array.from(document.querySelectorAll('.people p'));
+const people = [...document.querySelectorAll('.people p')]
```

Remove and object from an array of objects (very helpful in React/Redux)

```js
const comments = [
    { id: 209384, text: 'I love your dog!' },
    { id: 523423, text: 'Cuuute! 🐐' },
    { id: 632429, text: 'You are so dumb' },
    { id: 192834, text: 'Nice work on this!' },
];
//comment to remove
const id = 632429;
//find its index
const commentIndex = comments.findIndex(comment => comment.id === id);
//slice from the beggining of the array (index 0), up until the comment we want to remove, then 
//slice from from the index of the item that follows the one we want to remove.
const newComments = [...comments.slice(0,commentIndex), ...comments.slice(commentIndex + 1)];
```
In functions you can use it pass an array´s individual items as params

```js
const inventors = ['Einstein', 'Newton', 'Galileo'];
const newInventors = ['Musk', 'Jobs'];
inventors.push(...newInventors);
```

### the `...rest` param

The exact opposite of `...spread`, instead of unpacking arrays, it packs items into arrays. Its most commonly used as a destructuring assignment (Example from  [wes Bos](https://es6.io/)):

```js
const runner = ['Wes Bos', 123, 5.5, 5, 3, 6, 35];
const [name, id, ...runs] = runner;

const team = ['Wes', 'Kait', 'Lux', 'Sheena', 'Kelly'];
const [captain, assistant, ...players] = team;

```


[home][home] 

## Object Literal Upgrades

1. If the object property name and the variable you assigning it to have the same name you can skip the assignment:

    ```js
    const first = 'snickers';
    const last = 'bos';
    const age = 2;
    const breed = 'King Charles Cav';
    const dog = {
        firstName: first,
        last,
        age,
        breed,
        pals: ['Hugo', 'Sunny']
    };
    ```

1. When you have method definitiuons inside of an object:
    ```diff
    const modal = {
    -    create: function(param){
    -       /*some code*/
    -    }
    +    create(param) {
    +       /*some code*/
    +    }
    }
    ```
1. **Computed property names:** allow you to have an expression computed as a property name on an object.
    ```diff
    -function objectify (key, value) {
    -    let obj = {}
    -    obj[key] = value
    -    return obj
    -}

    +function objectify (key, value) {
    +    return {
    +        [key] : value
    +    }
    +}
    ```
    You can also do cool things like building object key/value pairs out of two arrays:
    
    ```js
    const keys = ['size', 'color', 'weight'];
    const values = ['medium', 'red', 100];

    const shirt = {
        [keys.shift()]: values.shift(),
        [keys.shift()]: values.shift(),
        [keys.shift()]: values.shift(),
    }
    ```

[home][home] 

## Promises

Promises are like an "I owe you" voucher for async operations, the most common scenario in the wild is when using `fetch`.

A promise has three states, `pending`, `fulfilled` and `rejected`, when [either of these options happens][mdnpromises]

>the associated handlers queued up by a promise's `then` method are called. (If the promise has already been fulfilled or rejected when a corresponding handler is attached, the handler will be called, so there is no race condition between an asynchronous operation completing and its handlers being attached.)
>
>As the `Promise.prototype.then()` and `Promise.prototype.catch()` methods return promises, they can be chained.

[It has 4 methods][mdnpromises]: 

1. `Promise.all(iterable)`
1. `Promise.race(iterable)`
1. `Promise.reject()`
1. `Promise.resolve()`

Example:

```js
//an http request using fetech will return a promise
const postsPromise = fetch('https://fake-data-base/blogs');

postsPromise
  .then(data => data.json())//when that promise is fulfilled, .then is called
  .then(data => { console.log(data) })//you can chain as many .then as you want 
  .catch((err) => {// and use .cathc to catch errors that occur anywhere i§n the chain
    console.error(err);
  })
```
You can also create your own promises:

```js
//the constructor takes 2 callback functions that ought to be self explanatory by now, Jon.
const p = new Promise((resolve, reject) => {
    setTimeout(() => { //async operation
        If(/*some condition*/){
            resolve(/*what you want to do*/)
        }else{
            reject(Error(/*your error message*/));
        }
    }, 1000);
});

p
    .then(data => {//what to do once promise is successful
        console.log(data);
    })
    .catch(err => {//or if t fails
        console.error(err);
    });
  
```

### Chaining promises

Example by [Wes Bos](https://es6.io/):

```js
const posts = [
  { title: 'I love JavaScript', author: 'Wes Bos', id: 1 },
  { title: 'CSS!', author: 'Chris Coyier', id: 2 },
  { title: 'Dev tools tricks', author: 'Addy Osmani', id: 3 },
];

const authors = [
  { name: 'Wes Bos', twitter: '@wesbos', bio: 'Canadian Developer' },
  { name: 'Chris Coyier', twitter: '@chriscoyier', bio: 'CSS Tricks and CodePen' },
  { name: 'Addy Osmani', twitter: '@addyosmani', bio: 'Googler' },
];

function getPostById(id) {
  return new Promise((resolve, reject) => {
    // find the post
    setTimeout(() => {
      const post = posts.find(post => post.id === id);
      if(post) {
        resolve(post);
      } else {
        reject(Error('Post not found!'));
      }
    },200);
  });
}

function hydrateAuthor(post) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const authorDetails = authors.find(person => person.name === post.author);
      if(authorDetails) {
        post.author = authorDetails;
        resolve(post);
      } else {
        reject(Error('Author not Found!'));
      }
    }, 200);
  });
}

getPostById(1)
  .then(post => {
    return hydrateAuthor(post);
  })
  .then(author => {
    console.log(author);
  })
  .catch(err => {
    console.error(err);
  })

```

### Working with multiple promises

You can use `Promise.all()` if you need several promises to resolve but each one is independent of the other. `.all` takes an array of promises and waits for all to be resolved.

According to [MDN][mdnpromises] the return values are:

>* An **already resolved** `Promise` if the *iterable* passed is empty.
>* An **asynchronously resolved** `Promise` if the *iterable* passed contains no promises. Note, Google Chrome 58 returns an **already resolved** promise in this case.
>* A pending `Promise` in all other cases. This returned promise is then resolved/rejected **asynchronously** (as soon as the stack is empty) when all the promises in the given *iterable* have resolved, or if any of the promises reject. Returned values will be in order of the Promises passed, regardless of completion order.

Example by [Wes Bos](https://es6.io/):

```js
const postsPromise = fetch('https://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');

Promise
    .all([postsPromise, streetCarsPromise])
    .then(responses => {
    return Promise.all(responses.map(res => res.json()))
    })
    .then(responses => {
        console.log(responses);
    });
```

[home][home] 

## Symbols

A new and confusing primitive type that serves as a unique identifier to avoid naming collisions when you create a property you want to be unique.

```js
const jon = Symbol('Jon');
const me = Symbol('Jon');

jon === me 
//false
jon == me
//false

const classRoom = {
    [Symbol('Mark')] : { grade: 50, gender: 'male' },
    [Symbol('olivia')]: { grade: 80, gender: 'female' }, //unique
    [Symbol('olivia')]: { grade: 80, gender: 'female' }, //unique
};
```

Symbols are non-iterable so you could also use them to store private data. In order to get access to the symbols of an object, we use `Object.getOwnPropertySymbols(obj)` which returns an array of the symbols (which act as the property keys).

```js
  const syms = Object.getOwnPropertySymbols(classRoom); //get the symbols
  const data = syms.map(sym => classRoom[sym]);// use them to map over the object and return the data
  console.log(data);
```

[home][home] 

## Modules

Modules are files with one or more functions, variables or data inside which you can make available to other files/applications. If you used React you are already familiar with them. Modules don't have good browser support yet so we need tooling in order to use them.

The syntax is composed of an `import` and an `export` statement.

There are `default` and `named` exports; the first one is normally used for the "main" feature of a module whereas the second one us commonly used for methods.

```js
//default export
//api.js
const apiKey = ´'abc123'
export default apiKey;

//app.js
import apiKey from './api'
```
When using default exports you can name import whatever you want so this would still import `apiKey`:

```js
//app.js
import randomStuff from './api'
```

Note that modules can **only have one** default export, they can however have many **named exports**. If using named exports, we need to use the exact same name as the export and wrap it in `{}`

```js
//api.js
export const apiKey = 'abc123'
//app.js
import { apiKey } from './api';
```

To export multiple named exports you can either wrap them in `{}`or use individual exports:

```js
export const apiKey = 'abc123';
export const url = 'https//random-stuff.com'
export function doSomething(){
    /*code*/
}
```
or

```js
const apiKey = 'abc123';
const url = 'https//random-stuff.com'
function doSomething(){
    /*code*/
}

export {apiKey, url, doSomething};
```

And you can import them similarly

```js
import { apiKey, url, doSomething} from './api';
//and change their names if you need to

import {apiKey as key, url, doSomething} from './api';
//and mix default with named exports
import aDefault, {apiKey as key, url, doSomething} from  './api';
```

[home][home]

## Classes


### Prototypal Inheritance review

```js
function Dog(name, breed) {
    this.name = name;
    this.breed = breed;
  }
  Dog.prototype.bark = function() {
    console.log(`Bark Bark! My name is ${this.name}`)
  }

  const snickers = new Dog('Snickers', 'King Charles');
  const sunny = new Dog('Sunny', 'Golden Doodle');

  Dog.prototype.bark = function() {
    console.log(`Bark Bark🇨🇦🇨🇦🇨🇦! My name is ${this.name} and I'm a ${this.breed}`);
  }

  Dog.prototype.cuddle = function() {
    console.log(`I love you owner!`);
  }
```

### ES6 Class

```js
class Dog {
    constructor(name, breed){
        this.name = name;
        this.breed = breed;
    }
    bark() {
        console.log(`bark bark my name is ${this.name}`)
    }
    cuddle(){
        /*code*/
    }
}
```
Classes also have **static methods**, which you can only call from the constructor (like `Array.of()` ). You declare them by adding `static`before the method.

```js
class dog{
    /*previous code*/
    static info(){
        /*stuff*/
    }
}
```


To add getters and setters you use `get` and `set` with the same syntax as `static`.

### Extending and `super`
To extend a class we use `super`:

```js

class Wolf extends Dog {
    constructor(name, breed, region{
      super(name, breed)
        this.region="Alaska"
    }
}
```
to do: modify/extend methods?

## Extend built-ins

You can extend built-ins, like an array to make custom collections, note that this **is NOT the same** as messing with the `prototype`, which you should avoid.

```js
  class MovieCollection extends Array {
    constructor(name, ...items) {
      super(...items);
      this.name = name;
    }
    add(movie) {
      this.push(movie);
    }
    topRated(limit = 10) {
      return this.sort((a, b) => (a.stars > b.stars ? -1 : 1)).slice(0, limit);
    }
  }

  const movies = new MovieCollection('Wes\'s Fav Movies',
    { name: 'Bee Movie', stars: 10 },
    { name: 'Star Wars Trek', stars: 1 },
    { name: 'Virgin Suicides', stars: 7 },
    { name: 'King of the Road', stars: 8 }
  );
```

[home][home]

## Generators

Generator functions allow us to create functions that we can start and stop and pass more information at run time, furthermore, **it will retain its scope until the last `yield`is called.** 

A generator function is created with `*`. **Calling a generator function doesn't execute its body**, instead it returns an `iterator` object on which we call `next()`. In turn, `next()` returns an object with two properties,  `value` and `done`. The former contains the yielded data and the later a boolean indication whether or not the generator has returned it´s last value.


```js
function* generator(i) {
  yield i;
  yield i + 10;
}
const gen = generator(10);
console.log(gen.next().value);
// expected output: 10
console.log(gen.next().value);
// expected output: 20

const secondGen = generator(15);
console.log(secondGen.next());
// expected output: {value: 15, done: false}
console.log(secondGen.next());
// expected output: {value: 25, done: false}
console.log(secondGen.next());
// expected output: {value: undefined, done: true}
```

You can programmatically add `yield` statements to a generator function using a `for..of` loop:

```js
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879 },
  { first: 'Isaac', last: 'Newton', year: 1643 },
  { first: 'Galileo', last: 'Galilei', year: 1564 },
  { first: 'Marie', last: 'Curie', year: 1867 },
  { first: 'Johannes', last: 'Kepler', year: 1571 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473 },
  { first: 'Max', last: 'Planck', year: 1858 },
];

function* loop(arr) {
  console.log(inventors);
  for (const item of arr) {
    yield item;
  }
}

const inventorGen = loop(inventors); //this just "starts" the generator but returns no value
console.log(inventorGen);
//expected value: loop {<suspended>}
inventorGen.next(); //this returns the first value!
//returns {value:{...}, done, false} where value is { first: 'Albert', last: 'Einstein', year: 1879 }
//5 more times
inventorGen.next();//7th
//returns {value:{...}, done, false} where value is { first: 'Max', last: 'Planck', year: 1858 }
inventorGen.next(); //No more values!
//returns {value: undefined, done: true} 
```

You can also loop through a `generator` using a `for...of` loop and it will return all of its values (example by Wes Bos):

```js
function* lyrics() {
    yield `But don't tell my heart`;
    yield `My achy breaky heart`;
    yield `I just don't think he'd understand`;
    yield `And if you tell my heart`;
    yield `My achy breaky heart`;
    yield `He might blow up and kill this man`;
  }

  const achy = lyrics();

  for (const line of achy) {
    console.log(line);
  }
  //logs
  /* 
But don't tell my heart
 My achy breaky heart
 I just don't think he'd understand
 And if you tell my heart
 My achy breaky heart
 He might blow up and kill this man
   */
achy.next();
//{value: undefined, done: true}
```

### Use cases

Generators can come in handy when dealing with waterfall ajax request in which each request requieres data from the previous one.

```js
//makes ajax requests and forwards data to the generator
function ajax(url) {
    fetch(url).then(data => data.json()).then(data => dataGen.next(data))
}
//the response then is stored inside the generator and available the rest of the yields
function* steps(name) {
  const student = yield ajax(`https://fakeuniversity.com/students${name}`);
  const academicRecord= yield ajax(`https://fakeuniversity.com/students/record${student[0][id]}`);
  const supervisorName = yield ajax(`https://fakeuniversity.com/supervisor/${adademicRecord[supervisor]}`);
}
//creates the generator
const dataGen = steps();
dataGen.next(); // kick it off
```

[home][home] 

## Proxies
From [MDN][mdnprox]:

>The Proxy object is used to define custom behavior for fundamental operations (e.g. property lookup, assignment, enumeration, function invocation, etc). You create a `proxy` with the constructor and passing it a `target` and a `handler`.
>
>`target`: A target object to wrap with Proxy. It can be any sort of object, including a native array, a function or even another proxy.
>
>`handler`:An object whose properties are functions which define the behavior of the proxy when an operation is performed on it.


```js
var p = new Proxy(target, handler);
```

```js
//the target, what we want to proxy
const person = { name: 'Wes', age: 100 };

const personProxy = new Proxy(person, {//we create the proxy and specify the target
  get(target, name) { //and pass the handler, in this case a trap for get
    return target[name].toUpperCase();//you choose what to return, in this case he is
    //converting the name property to upper case
  },
  set(target, name, value) {//passing another handler, in this case for set
    if(typeof value === 'string') {//checks the user is passing a string
      target[name] = value.trim().toUpperCase() + '✂️';//trims it and converts it upper case
    }
  }
});

personProxy.name = 'Wesley';
```
Essentially what the above example does is that you get in the way of the native gettter and setter and instead provide your own implementation, check [MDN's list](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Methods_of_the_handler_object) of the methods for which you can set a trap.

### Use Case

**Validation**

In this example you use a proxy to format phone numbers provided by a user, [click here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Validation) for an MDN example

```js
const phoneHandler = {
  set(target, name, value) {
    //gets only the numbers, so removes spaces, dashes etc
    target[name] = value.match(/[0-9]/g).join('');
  },
  get(target, name) {
    //applies a consistent format
    return target[name].replace(/(\d{3})(\d{3})(\d{4})/, '($1) $2-$3');
  }
}
//in this case we didn't start with an existing object but instead passed the proxy a blank one
const phoneNumbers = new Proxy({}, phoneHandler);
```

**Other use cases:**

[**Manipulating DOM nodes**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Manipulating_DOM_nodes): Sometimes you want to toggle the attribute or class name of two different elements.

[**Value correction and an extra property**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Value_correction_and_an_extra_property)

[**Finding an array item object by its property**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Finding_an_array_item_object_by_its_property) (This example can be adapted to find a table row by its cell. In that case, the target will be `table.rows`.)

At this point you might still be asking, if proxies are essentially getters and setters, why do we need them? If so, [read this](https://medium.freecodecamp.org/a-quick-intro-to-javascript-proxies-55695ddc4f98). Or read about other [use cases](https://medium.com/dailyjs/how-to-use-javascript-proxies-for-fun-and-profit-365579d4a9f8).

Either way, dont freak out because:

>the actual, real-world, practical good use cases for proxies are [few and far between](https://blog.logrocket.com/terrible-use-cases-for-javascript-proxies-2b585705da01). In most cases, the same thing can be achieved with a bit of repetitive boilerplate code with far better performance.


[home][home]

## Sets and WeakSets

 The biggest differences between a `set` and an `array` is that `sets` can only contain unique items, they are not indexed-based and you can´t access them individually. You can however, use a `for...of`.

 You create a new set by storing it in a variable and add items using `add`:

 ```js
const people = new Set();
people.add('Sofia');
people.add('Leo');
people.add('Jon');
//alternatively, you can pass the array-like object directly:
const people = new Set(['Sofia', 'Leo', 'Jon',])
 ```
 ### Methods

`size`: returns the size of the set (you **can't** use `.length`);

`delete([item])`: removes the given item and returns a boolean depending on successful deletion.

`has()`: checks if item exists, returns a boolean

`clear()`: clears the set.

`values()`: returns a set iterator that we can use to loop over with the `generator` API.

```js
//with generator
const peopleIt = people.values();
peopleIt.next();
//returns Object {value: 'Sofia', done: false}
//with for..of
```

## Example usage 

```js
//the set handles the list of guest to be seated
const brunch = new Set();
// as people start coming in you add them to the set
brunch.add('Jon');
brunch.add('Sofia');
brunch.add('Leo');
// when you are ready to sit them, you get the iterator
const line = brunch.values();
console.log(line.next().value); //everytime you call next, the guest that is up is
//returned and removed from the iterator (NOT FROM THE SET).
console.log(line.next().value);
brunch.add('Diego');//you can keep adding guest to the set anb they will be 
//available to the iterator
brunch.add('Hamilton');
console.log(line.next().value);
console.log(line.next().value);
console.log(line.next().value);
```

### WeakSets
A WeakSet can only contain objects, is not iterable which means it can’t be looped over and it doesn't have a `.clear()` method.

WeakSets take advantage of garbage collection, If you set an object to null, then you’re removing all references to it and the weakSet will get rid of it during garbage collection.

```js
let student1 = { name: 'James', age: 26, gender: 'male' };
let student2 = { name: 'Julia', age: 27, gender: 'female' };
let student3 = { name: 'Richard', age: 31, gender: 'male' };
const roster = new WeakSet([student1, student2, student3]);
student3 = null; //removes student3 from the weakset and memory
```

[home][home]

## Map and WeakMap

`Maps` are to objects what `sets` are to arrays; you store key-value pairs where both the keys and the values can be objects, primitive values, or a combination of the two. You use `set()` to pass key/value pairs:

```js
const employees = new Map();
employees.set('james.parkes@udacity.com', { 
  firstName: 'James',
  lastName: 'Parkes',
  role: 'Content Developer' 
});

employees.set('julia@udacity.com', {
  firstName: 'Julia',
  lastName: 'Van Cleve',
  role: 'Content Developer'
});

employees.set('richard@udacity.com', {
  firstName: 'Richard',
  lastName: 'Kalehoff',
  role: 'Content Developer'
});
``` 

### Methods

`delete(key)`

`clear()`

`has()`

`get()`

`keys()`

`values()`

Using both the `.keys()` and `.values()` will return a new iterator object, you can store that iterator object in a new variable and use `.next()` to loop through each key or value.  

If you use a `for of` loop, Instead, the key-value pair is split up into an array where the first element is the key and the second element is the value, and you can use destructuring ;)

```js
const members = new Map();

members.set('Evelyn', 75.68);
members.set('Liam', 20.16);
members.set('Sophia', 0);
members.set('Marcus', 10.25);

for(const [key, val] of members){
	console.log(`${key}:${val}`)
}

//returns
/* Evelyn:75.68
Liam:20.16
Sophia:0
Marcus:10.25 */
```

Your last option for looping through a Map is with the .forEach()

```js

members.forEach((value, key) => console.log(key, value));
 /* prints 'Evelyn' 75.68
 'Liam' 20.16
 'Sophia' 0
 'Marcus' 10.25
 */
```

### Example

One of the main advantages of a `map` over an object is that you can use objects as keys so you can use them as a sort of metadata dictionary.

In this example we, want to record every time a button is clicked,  and the count to still exist even if we remove the element.
So we use the button itself as a key of a map.


```js
//index.html
<body>
  <button>Snakes 🐍</button>
  <button>Cry 😂</button>
  <button>Ice Cream 🍦</button>
  <button>Flamin' 🔥</button>
  <button>Dancer 💃</button>
</body>
//app.js
  const clickCounts = new Map();
  const buttons = document.querySelectorAll('button');

//loop over each button and add it to the map
  buttons.forEach(button => {
    clickCounts.set(button, 0);
    //add the event listener to the button
    button.addEventListener('click', function() {
      //get the current click count
      const val = clickCounts.get(this);
      //increment it by 1
      clickCounts.set(this, val + 1);
      console.log(clickCounts);
    });
  });

```

### WeakMap

A WeakMap is just like a normal Map with a few key differences; it can only contain objects as keys, is not iterable which means it can’t be looped and it does not have a `.clear()` method.

[home][home] 

## Async + Await Flow Control

`Async/await` is built on top of `promises` and it cant be used with callbacks, an async function is, like promises, non blocking and allows you to writte async code that looks and behaves more like synchronous code, note Async is essentially syntactic sugar for promises.
Async function are created by prepending `async`befor e afunction declaration and can be paused with `await`, which **can only be used inside an `async` function**.

```js
 //with promises
async function go() {
  const p1 = fetch('https://api.github.com/users/wesbos');
  const p2 = fetch('https://api.github.com/users/stolinski');
    // Wait for both of them to come back
  const res = await Promise.all([p1, p2]);
  const dataPromises = res.map(r => r.json());
  const [wes, scott] = await Promise.all(dataPromises);
  console.log(wes, scott);
}
go();

//with async
async function getData(names) {
  const promises = names.map(name => fetch(`https://api.github.com/users/${name}`).then(r => r.json()));
  const people = await Promise.all(promises);
  console.log(people);
}

getData(['wesbos', 'stolinski', 'darcyclarke']);
```

Need more examples? Check [this article/tutorial](https://medium.freecodecamp.org/how-to-master-async-await-with-this-real-world-example-19107e7558ad).


[home][home] 

## ES7 and Beyond

### Class Properties

If you'v used React you have probably already used them.

```diff
class Dog {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
-   this.barks=0
-   this.bark=this.bark.bind(this)
  }
//now every instance of the dog class will have the barks property and you don´t
//need to bind the methods to it.  
+  barks = 0;

  bark() {
    console.log(`Bark Bark! My name is ${this.name}`);
    this.barks = this.barks + 1;
    console.log(this.barks);
  }
}
```

### `padStart` and `padEnd`

Padd the start or end of string if its not the length you want.

```js
const strings = ['short', 'medium size', 'this is really really long', 
'this is really reall really really really really long'];
//sorts them by length
const longestString = strings.sort((a, b) => b.length - a.length).map(str => str.length)[0];
//padds the all the strings to match the length of the longest string
strings.forEach(str => console.log(str.padStart(longestString)));
```
### `Array.includes()`

```js
var array1 = [1, 2, 3];
console.log(array1.includes(2));
// expected output: true
```

### Exponential operator
```js
//three to the power of three used to be
Math.pow(3, 3)
//now
3**3
//returns 27
//and you can chain them
2**2**2
//returns 16
```
### Function arguments trailling coma

Function arguments can end with a coma. This is helpfull when collabnorating with other people so that version control doesnt credit you with modifying the previous line if have to add a coma to add another argument.

```js
function family(
      mom,
      dad,
      children,
      dogs,
    ) {
//**stuff*/
    }
```

### Object.entries() and Object.values()

New static methods that returns an array of arrays with key/value pairs, in the case of `entries` or array of the values.

```js
const inventory = {
      backpacks: 10,
      jeans: 23,
      hoodies: 4,
      shoes: 11
};

Object.values(inventory)
//expected return
[10, 23, 4, 11]

Object.entries(inventory)
//exxpected return
[
  ["backpacks", 10],
  ["jeans", 23],
  ["hoodies", 4],
  ["shoes", 11]
]
```

[home][home] 

[home]:#table-of-contents

[mdnpromises]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[mdnprox]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy

## to do/ look into:

* [ ] Beyond console.log (console.table)
* [ ] arguments object
* [ ] polyfill.io as an alternative to Babel.
* [ ]setters and getters 