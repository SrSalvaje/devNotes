# React Notes

## Table of contents

* [Steps for developing a react app](#development-steps).
* [State Criteria](#state-criteria).
* [Modifying State](#modifying-state).
* [Useful Patterns](#useful-patterns)



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

[home][home]

## Modifying State

setState is asynchronous so you should´t modify it based on its current value, to do so you can use setState with a function that returns a state object. In the following example, the next value of `counter` depends on its previous value:

```js
this.setState((prevState, props) => (
        {counter: prevState.counter += 1 }
        )
    );
```
Read more:
[using a function in setState](https://medium.com/@wisecobbler/using-a-function-in-setstate-instead-of-an-object-1f5cfd6e55d1), [Functional setState is the future of React](https://medium.freecodecamp.org/functional-setstate-is-the-future-of-react-374f30401b6b)

State is **inmutable** so you **CAN´T** modify array objects directly:

```js
this.setState({anArray: this.state.anArray.push(value)});
```
Instead, use `Array.concat()` or `Object.assign`:
```diff
- this.setState({anArray: this.state.anArray.push(value)});
+   const clonedArray = this.state.anArray.concat(value);
+    this.setState({anArray:clonedArray});
```
In the case of Object.assign:

```js
    const clonedObject = Object.assign({}, this.state.anObject, {/* properties to modify/add */});
```

The first argument is a new JavaScript object (which in this case its an empty object), the one that Object.assign() will ultimately return. The second is the object whose properties we’d like to build off of. The last is the changes we’d like to apply

If you need **something to happen right after you modified state**, you can provide a callback function:

```js
this.setState({myState:newValue}, this.callbackFunction);
```

[home][home]

## Useful Patterns
**Double bangs** `!!` return the boolean true/false association of a value:

```js
!!'abc'; //evaluates to true
!!null; //evaluates to false
```

[home][home]









[home]:#table-of-contents