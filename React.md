# React Notes

## Table of contents

-   [Steps for developing a react app](#development-steps).
-   [State Criteria](#state-criteria).
-   [Modifying State](#modifying-state).
-   [Useful Patterns](#useful-patterns).
-   [Testing](#testing)

## Development Steps

Based on [Thinking in React](https://reactjs.org/docs/thinking-in-react.html)

1. Break the app into components.
1. Build a static version of the app.
1. Determine what should be [stateful](#state-criteria).
1. Determine in which component each piece of state should live.
1. Hard-code initial states.
1. Add inverse data flow.
1. Add server communication.

[home][home]

## State Criteria

1. Is it passed in from a parent via props? If so, it probably isn’t state.
2. Does it change over time? If not, it probably isn’t state.
3. Can you compute it based on any other state or props in your component? If so, it’s not state.

To determine in which component each piece of state should live:

-   Identify every component that renders something based on that state.
-   Find a common owner component (a single component above all the components that need the state in the hierarchy).
-   Either the common owner or another component higher up in the hierarchy should own the state.
-   **If you can’t find a component where it makes sense to own the state**, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component.

[home][home]

## Modifying State

`setState` is asynchronous so you should´t modify it based on its current value, to do so you can use `setState` with a function that returns a state object. In the following example, the next value of `counter` depends on its previous value:

### With a Function

```js
this.setState((prevState, props) => ({ counter: (prevState.counter += 1) }));
```

Read more:
[using a function in setState](https://medium.com/@wisecobbler/using-a-function-in-setstate-instead-of-an-object-1f5cfd6e55d1), [Functional setState is the future of React](https://medium.freecodecamp.org/functional-setstate-is-the-future-of-react-374f30401b6b)

### Treat state as inmmutable

You **SHOULDN´T** modify array/objects directly:

```js
this.setState({ anArray: this.state.anArray.push(value) });
```

Instead, use `Array.concat()` or `Object.assign` to copy them:

```diff
- this.setState({anArray: this.state.anArray.push(value)});
+   const clonedArray = this.state.anArray.concat(value);
+    this.setState({anArray:clonedArray});
```

In the case of Object.assign:

```js
const clonedObject = Object.assign({}, this.state.anObject, {
    /* properties to modify/add */
});
```

The first argument is a new JavaScript object (which in this case its an empty object), the one that Object.assign() will ultimately return. The second is the object whose properties we’d like to build off of. The last is the changes we’d like to apply.

**Important!**

Keep in mind that if the object/array has references to other objects/arrays, you also need to make a copy of those objects before modifying them.

```diff

const hello = {
   a:"hello"
};

const goodbye = {
   a:"goodbye"
};

const myArrayOfObjects = [hello, goodbye];


-let copyOfMyArray = myArrayOfObjects.map( (obj) => {
-    obj.a="ups, modifying original object!"
-})



console.log(hello);
//Expected return
//{a: "ups, modifying original object!"}

+let copyOfMyArray = myArrayOfObjects.map( (obj) => ()
+    Object.assign({}, obj, {a:"now you are modifying a +copy!"})
+))

console.log(hello);
//Expected return
//{a: "hello"}

console.log(copyOfMyArray);
//(2) [{…}, {…}]
//0: {a: "now you are modifying a copy!"}
//1: {a: "now you are modifying a copy!"}

```

### With a Callback Function

If you need **something to happen right after you modified state**, you can provide a callback function:

```js
this.setState({ myState: newValue }, this.callbackFunction);
```

-   stuff
-   adding

[home][home]

###JSX Gotchas

-   You cant use `class` and `for` attributes
    -   instead use `className` and `htmlFor`
-   Entities and Emojis (`<`, `>`, copyright symbol).
    -   To use them either place the code in literal text or unicode:
        ```js
        < li > phone : & phone ; < /li>   //literal
        < li > phone : { '\u0260e' } < /li> //unicode
        ```
-   Custom attributes; If we want to apply our own attributes that the HTML spec does not cover, we have to prefix the attribute key with the string `data-`.

    ```js
    <div className="box" data-dismissible={true} />
    ```

*   Accesibility, just preppend `aria-` to the key
    ```js
    <div aria-hidden={true} />
    ```

[home][home]

## Context

1. Create context

```js
//context.js
import React from 'react';

export const themes = {
    light: {
        foreground: '#222222',
        background: '#e9e9e9'
    },
    dark: {
        foreground: '#fff',
        background: '#222222'
    }
};

export const ThemeContext = React.createContext(themes.dark);
```

1. Set context Provider

```js
//App.js
//..
import { ThemeContext, themes } from './theme';

//..
state = { theme: themes.dark };

//..
return (
    <div className="App">
        <ThemeContext.Provider value={this.state.theme}>
            <Header />
            <p className="App-intro">
                To get started, edit <code>src/App.js</code> and save to reload.
            </p>

            <button onClick={this.changeTheme}>Change theme</button>
        </ThemeContext.Provider>
    </div>
);
```

1. Consume context

```js
//header.js
//..
import { ThemeContext } from './theme';

export const Header = props => (
    <ThemeContext.Consumer>
        {theme => (
            <header
                className="App-header"
                style={{ backgroundColor: theme.background }}
            >
                <img src={logo} className="App-logo" alt="logo" />
                <h1 className="App-title" style={{ color: theme.foreground }}>
                    Welcome to React
                </h1>
            </header>
        )}
    </ThemeContext.Consumer>
);
```

## syntax cheatsheet and Useful Patterns

### Render Prop

A component with a render prop takes a function that returns a React element and calls it instead of implementing its own render logic.

### defaultProps

```js
class Counter extendes React.Component {
   static defaultProps = {
      initialValue:1
   };
   //...
}
```

Then when you use the component:

```diff
+<Counter/>
-<Counter initialValue={1}/>
```

### Double bangs

`!!` return the boolean true/false association of a value:

```js
!!'abc'; //evaluates to true
!!null; //evaluates to false
```

### JSX spread syntax

```diff
const props = { msg : "Hello" , recipient : "World" }
-< Component msg = { "Hello" } recipient = { "World" } />
+< Component {... props } />
```

### Pass multiple classes

```js
let cssNames = ['box', 'alert'];
// and use the array of cssNames in JSX
<div className={cssNames.join(' ')} />;
```

### Modifying data on `setState`

```js
//We can call map() on this.state.timers from within the JavaScript object we’re passing to setState() .
this.setState({ timers: this.state.timers.map((timer) => {
     if (timer.id === attrs.id) {
         return Object.assign({}, timer, { title: attrs.title, project: attrs.project, }
);
```

[home][home]

## Testing with Jest

-   Try to focus on testing the UX and not the dom structure.

### Unit

A basic unit test in `jest` looks like this:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import ItemList from '../item-list';

test('title', () => {
    const container = document.createElement('div');

    ReactDOM.render(<ItemList items={[]} />, container);

    expect(container.textContent).toMatch('no items');
});
```

### Integration

### Intent

### Look into

-   visual regression testing
    -   puppeteer + jest?
-   Flow-typed

[home][home]

Look into:

getDeafultProps
getInitialState

[home]: #table-of-contents
