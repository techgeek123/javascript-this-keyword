# JavaScript - This:Intro,ImplicitBinding,Call, Apply, Bind, New

## Learning Competencies

By the end of this chapter, you should be able to

- Explain what the keyword `this` is and what a call-site is
- Explain how the keyword `this` is determined with default,implicit binding
- Discuss this difference between the default and implicit binding of the keyword `this`
- Compare and contrast the parameters and return values of `call`, `apply`, and `bind` and changing the context of `this` using `call`, `apply`, and `bind`
- Explain what the new keyword does and when it is used


## Overview

## Wiki Says
> In JavaScript, which is a programming or scripting language used extensively in web browsers, `this` is an important keyword, although what it evaluates to depends on where it is used.
- When used outside any function, in global space, `this` refers to the enclosing object, which in this case is the enclosing browser window, the `window` object.
- When used in a function defined in the global space, what the keyword `this` refers to depends on how the function is called. When such a function is called directly (e.g. `f(x)`), this will refer back to the global space in which the function is defined, and in which other global functions and variables may exist as well (or in strict mode, it is undefined). If a global function containing this is called as part of the event handler of an element in the document object, however, this will refer to the calling HTML element.
- When a method is called using the `new` keyword (e.g. `var c = new Thing()`) then within Thing `this` refers to the Thing object itself.
- When a function is attached as a property of an object and called as a method of that object (e.g. `obj.f(x)`), `this` will refer to the object that the function is contained within. It is even possible to manually specify this when calling a function, by using the `.call()` or `.apply()` methods of the function object. For example, the method call `obj.f(x)` could also be written as `obj.f.call(obj, x)`.

The keyword `this` is one of the more unique features of JavaScript. It often presents a stumbling block to people learning JavaScript, even if they already know some other language. However, the rules governing this are relatively straightforward once you've familiarized yourself with them.

At its core, `this` is just a keyword in JavaScript that refers to some object. For instance, if you hop into the Chrome console and type `this`, you'll see that it refers to the `window`. However, the value of the keyword `this` depends on where in your code you use it. Let's take a look at the different ways the value for the keyword this gets set

The terms `default binding`, `implicit binding`, `explicit binding`, and `new binding` come from the incredible book You Don't Know JS by Kyle Simpson. You can read more about these terms in his book on prototypes and the keyword `this` [here](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md).

### Call-site

To understand `this` binding, we have to understand the call-site: the location in code where a function is called (**not where it's declared**). We must inspect the call-site to answer the question: what's *this* `this` a reference to?

Finding the call-site is generally: "go locate where a function is called from", but it's not always that easy, as certain coding patterns can obscure the *true* call-site.

What's important is to think about the **call-stack** (the stack of functions that have been called to get us to the current moment in execution). The call-site we care about is *in* the invocation *before* the currently executing function.


### Default binding 

You can think of `this` as the last possible option when all other rules do not apply. The default binding exists when the keyword `this` is in the global context. As we've already seen, in the case of the browser the keyword this has a default binding to the `window`

For the other three bindings, the value of the keyword this is set to some value other than the default when it is used inside of a function. But the details of how the value gets set depend on the **call-site** of the function. The call-site is simply the way and location in which the function is called.

There are two modes Default and Strict.You can read more about strict mode [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).

### Implicit Binding

One option is that the function wrapping the keyword `this` is being called as a method on some object. In this case the call-site has some parent object, and according to the implicit binding rule, `this` should refer to this parent object. Anytime you see the keyword `this`, you should check to see whether the closing function has an associated parent object when the function is called. In other words, check the call-site!

### Explicit Binding
It is possible to call a function using the call() or apply() methods, which are very similar except for the parameters they take in. When a function is called through these, we are explicitly binding its execution context to an object.

What call() (or apply()) does is explicitly tell the function what object to use as this

#### When to use each? `call`, `apply`, and `bind`
> [Call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) and [apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) are pretty interchangeable. Just decide whether it’s easier to send in an array or a comma separated list of arguments.

> I always remember which one is which by remembering that Call is for 'C'omma (separated list) and Apply is for 'A'rray.

> [Bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) is a bit different. It returns a new function. Call and Apply execute the current function immediately.

> Bind is great for a lot of things. We can use it to [curry functions](https://www.sitepoint.com/currying-in-functional-javascript/). We can also use it for events like onClick where we don’t know when they’ll be fired but we know what context we want them to have.

We mentioned four ways to determine the value of the keyword `this`. Here's a bit more detail on each type of binding:

1. Default binding - catch-all rule, when the keyword `this` is in the global context
2. Implicit Binding - when the keyword `this` is attached to a parent object
3. Explicit Binding - when we want to explicitly set the context for the keyword `this`
4. The `new` keyword - when the `new` keyword is used, the keyword `this` refers to an object that is created from a function after the `new` keyword (usually called a *constructor function*).

### New Binding

#### Wiki Says
> The `new` operator can be used to create an object wrapper for a Boolean primitive. However, the `typeof` operator does not return boolean for the object wrapper, it returns object. Because all objects evaluate as true, a method such `as .valueOf()`, or `.toString()`, must be used to retrieve the wrapped value. Follow [Wiki](https://en.wikipedia.org/wiki/JavaScript_syntax) for more

#### The `new` keyword
So far we have seen three ways for the value of the keyword `this` to be determined. We have seen the default (or global) binding, implicit (or object) binding, and explicit binding (with call, apply, and bind). Now let's explore the last way the keyword `this` can be determined: with the `new` keyword.

Let's take a look at some code:
```js
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}
```
The first thing you might notice here is that the name of the function is capitalized. This will not effect the function in any way, but is a signal to ourselves and other developers that this is a certain kind of function, a "constructor" function.

We will learn more about constructor functions in the next section, but constructor functions serve the purpose of creating or "constructing" objects that will be created using the `new` keyword.

In the body of the function, we add two properties on the keyword `this`, `firstName` and `lastName`, and assign them to the values that are passed to the function.

When we invoke this function, what should we expect to see?
```js
var elie = Person('Elie', 'Schoppik');
elie; // undefined
```
Why did this happen? Remember that functions which do not return any values return `undefined`, so nothing special is happening here. And what about the value of the keyword `this`?

In this case, there's no explicit binding going on, and `Person` isn't being called as a method on some parent object. So the default binding must take hold, which means that `this` will be defined as the `window` object. What we have actually done is defined two properties on the `window`, `firstName` and `lastName`!
```js
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

window.firstName; // undefined
window.lastName; // undefined

var elie = Person('Elie', 'Schoppik');
elie; // undefined

window.firstName; // 'Elie'
window.lastName; // 'Schoppik'
```
So how do we fix this issue and change the value of the keyword `this`? We use the `new` keyword. Let's try using it to invoke the `Person` function and examine the value returned.
```js
var elie = new Person('Elie', 'Schoppik')
elie // {firstName: 'Elie', lastName: 'Schoppik'}
```
So what did the `new` keyword do? It somehow allowed us to return an object with the values specified! In more detail here is what the `new` keyword does:

1. It creates an empty object.
2. It sets the value of the keyword `this` to be that empty object created.
3. It adds an implicit `return this` in the function. Notice we are not using the word `return` in our function, and yet our function is still returning an object when we use the new keyword!

The last thing the `new` keyword does is a bit more complex and we will analyze it in much more detail in the next section. The `new` keyword creates a link between the `prototype` property on the constructor function and the object that was just created. Once again, we will examine this link in much more detail so do not worry about understanding this final point for now.

## Resources

- Check out an excellent tutorial on **`this` keyword** from *MDN* [here](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/this)
- An amazing blog **Gentle explanation of 'this' keyword in JavaScript** by *Dmitri Pavlutin*. Read [here](https://rainsoft.io/gentle-explanation-of-this-in-javascript/)


- Follow this amazing tutorial by *CodePlanet* [here](https://codeplanet.io/javascript-apply-vs-call-vs-bind/)
- See this [video](https://egghead.io/lessons/javascript-the-this-keyword-introduction-and-implicit-binding) tutorial on `this` keyword and **implicit binding**.
- Read [this](http://fitzgeraldnick.com/2010/05/20/javascript-bind-and-this.html) blog by *Nick Fitzgerald* on **`this` and `bind`.**
- Read [this](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md) amazing chapter from the book *You don't know JS*. 


- Read [this](https://javascriptplayground.com/blog/2012/12/the-new-keyword-in-javascript/) amazing tutorial on **`new` keyword** from *JS Playground*. 
- Check out tutorial **`new` keyword in JavaScript** from *TuorialsTeacher.com*. Read [here](http://www.tutorialsteacher.com/javascript/new-keyword-in-javascript)
- Read [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) excellent tutorial on **`new`** from *MDN*. 
