# TODO

* Iterables
* Async functions - push/pull generators
* Event loop / message queue

# All Versions
## Object Literal vs. JSON
* They are not the same thing.
* JSON requires property names to be strings: `"property": ...`
* JSON values do not accept named references, their values are literal. Object literals can hold variables/functions etc.

## Console

### Compose Log Objects
Using computed property names, you can log variables/objects as a single object - in a single log.

```js
console.log({ foo, bar, baz})
// {foo: 1, bar: 2, baz: 3}
// foo: 1
// bar: 2
// baz: 3
```


### Use Custom Styles

`%c` indicates the intent to use css, the second arg is the styles.

```js
console.log('%c MSG', 'color: red; font-weight: bold');
```


### Table

For objects that have common properties, you can display them as a table:

```js
foo = { a: 1, b: 2, c:3 };
bar = { a: 4, b: 5, c:6 };
baz = { a: 7, b: 8, c:9 };

console.table([foo, bar, baz])
```

### Log Time

You can use `console.time()` and `console.timeEnd()`, to time some code.

### Trace

You can perform a stack trace with `console.trace()`. It will tell you where it was called.

```js
func someFunc() {
	console.trace('msg');
	// do stuff.
}
```


## This, That, Whatever

### Bind() ([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind))

Bind can set the execution context. This doesn't work with arrow functions.

```javascript
const foo = {
  bar: 1,
  getBar: function () {
    return this.bar;
  },
}

foo.getBar.bind(foo)() // Bind just binds, you need to call the function too.
// 1
```

### Arrow Functions ([Blog](https://derickbailey.com/2015/09/28/do-es6-arrow-functions-really-solve-this-in-javascript/))

The context for arrow functions is always **where it is defined** - aka "lexical scoping".

They are functions, and never methods.

# ES6

## Arrays

### Reduce

Accumulate against each value in an array. Reduce accepts a callback, that can be set to perform any logic required, to accumulate a value in any specific way. This is an alternative to looping, and performing manual arithmetic.

```
const accum = someArr.reduce((accum, current) => {...})
```


### Spread Syntax

```javascript
const foo = [1, 2, 3];
const bar = [4, 5, 6];
const foobar = [...foo, ...bar]
```

## Computed Property Names

Property names that are computed at runtime

```
const bar = 'akey';
const foo = {
	[bar]: 'aval',
}
foo.akey
// aval
```

## Destructuring
### Arrays

#### Arguments

```javascript
function foo ([ one, two, three ]) {
  // one, two, three are separate vars.
};
foo([1, 2, 3]);
```

#### Default Values

```javascript
const [ a=1, b=2 ] = [100];
// >>> a === 100; b === 2
```

#### Indicies

```javascript
const [a, , b] = [a, x, b];
```

#### Rest Syntax (h::t)

```javascript
const [ a, ...b ] = [1, 2, 3];
// Similar to h::t in OCaml.
```

#### Swapping Values

```javascript
let [ a, b ] = [ b, a ];
```

#### Unpacking

```javascript
const [ one, two, three ] = [1, 2, 3];
// >>> one === 1; two === 2, three === 3;
```

#### Tips
* Return values from functions can be arrays.
* Regex matches can come in the form of arrays.
* Function arguments can be destructured, or be in the form of arrays.
* You can combine array and object destructuring.

---

### Object Literals
#### Defaults Values

```javascript
const { a = 100, b = 200 }  = {a: 1};
// >>> a === 1; b === 200;
```

#### Nested Objects

```javascript
// The syntax is nearly the same as a nested object literal.
const { a, b: { x } } = { a: 1, b: { x: 'x' } };
// >>> x === 'x';

// Reasignments are possible too
const { a, b: { x: renamed } } = { a: 1, b: { x: 'x' } };
// >>> renamed === 'x';

```

#### Reasignment

Reassign values to different names. Also, note that default values can also apply.

```javascript
const {a: aa, b: bb = 5} = {a: 3};
// >>> aa === 3; bb === 5;
// a and b do not exist.
```

#### Rest Syntax (h::t)

```
const {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40}
// >>> a === 10; b === 20; rest === {c: 30, d: 40};
```

#### Simple Destructuring

```javascript
const foo = {
  bar = 'bar',
  baz = 'baz',
};
const { bar, baz } = foo;
// >>> bar === 'bar'; baz === 'baz';
```

```javascript
// Another example.
const { a, b }  = {a: 1, b: 2};
```

Use in cases where the object has a very large name.

#### Tips

* Objects can be passed as arguments, and be destructured.
* You can combine array and object destructuring.


## Generators

* Generator functions return an Iterator object when initially called: `let iter = someGenerator();`
* Iterators implement the Iterator protocol - e.g. `next();`
 

```javascript
function* foo() {
	yield 1;
	yield 2;
}

let iter = foo(); // Although the shorthand seems to also work: e.g. (let bar of foo())
for (let bar of iter) { // here.
	console.log(bar);
}

// 1
// 2
```

### The Actual Yielded Value

Interestingly you can see the actual yielded return value when calling next() directly - which is an object, with a 'done' key:

```javascript
function* foo() {
	yield 1;
	yield 2;
}

let iter = foo();

console.log(iter.next());
console.log(iter.next());
console.log(iter.next()); // There isn't a third yield

// Object { value: 1, done: false }
// Object { value: 1, done: false }
// Object { value: undefined, done: true } // Yielded an undefined value.
```

### Return

You must '*return*' an explicit value to signal the end of the generator.

```javascript
function* foo() {
	yield 1;
	yield 2;
	return 3;
}

let iter = foo();

console.log(iter.next());
console.log(iter.next());
console.log(iter.next()); // There isn't a third yield

// Object { value: 1, done: false }
// Object { value: 1, done: false }
// Object { value: 3, done: true } // Return sets done to true

// Pulling any more values results in { value: undefined, done: true }
```

## Promises
### Example

```javascript
const promise = new Promise((resolve, reject) => {
  // Async like this (preferrably).
  async().then((retval) => {
    // resolve() or reject() based on retval.
  })
  
  // Or this..
  async2(resolve, reject); // If callbacks are accepted as args.
})
.then(successCallback, failureCallback) // failureCallback (optional) executes on reject().
.then((retval) => { // Previous callbacks can return a Promise, allowing you to chain then().
  // This successCallback executes only if resolve() has been executed.
  // Success/failure callbacks can take the retval as an arg.
  // retval is defined by resolve(retval); reject(retval);
})
.catch((e) => { // Takes callback.
  // Catch all rejections (e.g. executes only on reject() ).
  // Mutually exclusive wtih failureCallbacks.
  // 'e' is defined by reject(e) - conventionally an error object.
}).finally(() => { // Takes callback.
  // Guarenteed to run last.
  // Takes no args, but returns a new Promise.
})
.then((retval) => {
  // This is a new Promise.
  // This will run only when the prior Promise chain has completed.
  // 'retval' is the return value from the last then().
});
```

---

### Resolve

This should be called when an async function completes within the Promise definition - typically this async function will accept a callback, or return a Promise type. In either case, you can attach a callback - e.g. resolve(), or reject().

### Reject

This should be called when there's a failure. Typically like so: `reject(new Error('msg'))`.

```javascript
new Promise((resolve, reject) => {
  async().then((retval) => {  // Async here, returns its own Promise.
    if (/* Good stuff */) resolve();
    else reject();
  })
});

// Or..
new Promise((resolve, reject) => {
  async2(resolve, reject); // If async2 accepts it.
});
```

---

### Catch

Catch is executed on reject(arg), and is **mutually exlusive with failureCallbacks**.

The argument to `reject(arg)` can be anything, but conventionally it's an error object, and it's passed to catch's callback.

```javascript
.catch((e) => {
  // 'e' is reject(e);
});
```

#### Mutually Exclusive Catch/Callbacks

Defining a failureCallback in then() means that .catch() won't execute on rejection.

```javascript
.then(null, failureCallback)
.catch((e) => {
  // This won't execute.
})
```

---

### Chaining

Promise objects can be chained, because then() returns a Promise.

The explicit return value within the body of then() can be anything you want (perhaps another Promise), but the value passed into the next `then(callback)`, is auto-flattened, unwrapped, and is never received as a Promise type - e.g. you cannot call `.then()` on it, because it has been consumed.

```javascript
Promise()
  .then((retval) => {
    // My retval comes from the Promise.
  }) // Then returns a Promise.
  .then((retval) => { // But retval isn't necessarily a Promise.
    // My retval comes from the previous then(),
    // It will never be a Promise type, as the Promise has been consumed.
  })
```

---

### Promise Tips
* Callbacks are never called before the completion of the current JS event loop - e.g. then() is called within a later iteration of the event loop from the Promise()
* `resove()` and  `reject()` can be attached to events, enabling truly async code.
* Always return results, use `() => {}` to implicitly enforce this.
* Callbacks can return a Promise, allowing chaining of then() functions.
* ``then(success, failure)` .. either callback is optional:
  * ``then(null, failure);` or `then(success);`.
  * `then(null, failure)` is a more granular approach to `.catch(retval)`.

## Shorthand
### Object Literals

```javascript
const baz = 'baz';
const foo = {
  bar: 'bar',
  baz, // No need for baz: baz,
}
```

### Spread Syntax

#### Objects

You can merge objects like so (objects on the right take priority, and overwrite when there's name collisions with the properties):

```js
const foo = {a: 1};
const bar = {b: 2};
const baz = {...foo, ...bar};
// baz = {a: 1. b: 2}
```

