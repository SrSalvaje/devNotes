## `This`

From Kyle Simpson's ["this & OBJECT PROTOTYPES"](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch1.md):

> We said earlier that `this` is not an author-time binding but a runtime binding.
> It is contextual based on the conditions of the function's invocation. `this`
> binding has nothing to do with where a function is declared, but has instead everything to do with the manner in which
> the function is called. [...]

> `this` is neither a reference to the function itself, nor is it a reference to > the function's lexical scope.
> `this` is actually a binding that is made when a function is invoked, and what it
> references is determined entirely by the call-site where the function is called.
> Determining the `this` binding for an executing function requires finding the
> direct call-site of that function. Once examined, four rules can be applied to
> the call-site, in this order of precedence:
>
> 1.  Called with new? Use the newly constructed object.
> 1.  Called with call or apply (or bind)? Use the specified object.
> 1.  Called with a context object owning the call? Use that context object.
> 1.  Default: undefined in strict mode, global object otherwise.

Rebase Practice:

Now I´v also modified this
