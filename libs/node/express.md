## Middleware ([Docs](https://expressjs.com/en/guide/using-middleware.html))

* `req.app` and `res.app` both hold a reference to the main app object, and can be used to access its members.
* Middleware is mounted with app.use, and app.METHOD: app.post, app.get, app.put, app.delete

### App Level vs Router Level ([Docs](https://expressjs.com/en/4x/api.html#router))

App level middleware and router level middleware are similar but separate. Router level middleware focuses on routing, but acts similar to app level middleware - so it can be used with `app.use`. 

### A Middleware Stack

A middleware stack is: `app.use(func1, func2, func3)`, sometimes referred to a sub-stack if it's mounted ontop of another route.

<details>
	<summary>Substack Example</summary>

```javascript
app.use(someFunc); // Mounted at '/'
app.use('/foo', func1, func2, func3); // A substack
```

'/foo' is mounted on top of /, and that stack is local to *that* path  - so it's a substack.

</details>

### next()

next() can call the **next function** in the middlware stack, or the **next route**.

<details>
	<summary>Examples</summary>

Calling the next function in the middleware stack:

```javascript
app.get('/', (req, res, next) => {
	next();  // This will call func2
}, funcTwo, (req, res, next) => req.send('ok'))
```

Calling the next route:

```javascript
app.get('/', (req, res, next) => {
	next('route');  // Use 'route' as an arg, but only works for app.METHOD middleware
}, (req, res, next) => req.send('This does not get executed...'))

// This is the next route - both routes are mounted on '/'.
app.get('/', (req, res, next) => res.sent('...but this does'));
```

</details>

### Grouping Middleware

Use arrays

```javascript
const doStuff = [func1, func2, func3];
app.use(doStuff, (req, res, send) => ...);
```

### Error Handling ([Docs](https://expressjs.com/en/guide/error-handling.html))

You must provide EXACTLY four arguments to define a *custom* error handling middleware function.

```js
app.use((err, req, res, next) => {...})
```

Use `next(Error())` to trigger it (from within a non-error middleware function).

#### The Default Error Handler

A default error handler is added to the end of the middleware stack, and is triggered if a custom error handler is not defined.

You can still call the default handler within your custom handler, by simply passing the error object into next: `next(err)`

<details>
	<summary>Example</summary>
	
```js
app.use((err, req, res, next) => { // A custom error handler.
	// stuff
	next(err); // Calls default handler.
})
```
</details>

## Response Object

### 'End' Calls
`send()`, `json()`, ... etc. all end the stream

```javascript
res.send('Foo');
```

`.end()` can be called explicitly otherwise

```jjavascript
res.status(200).end()
```

### Order Matters
status() before 'end'() calls.

```javascript
res.status(404).send('Not Found'); // Works!
res.send('Not Found').status(404); // Does not work.
```
