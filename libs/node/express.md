## Response Object

### End Type Calls
`send()`, `json()`, ... etc. all end the stream

```js
res.send('Foo');
```

`.end()` can be called explicityly otherwise

```js
res.status(200).end()
```

### Order Matters
status() before end() type calls.

```
res.status(404).send('Not Found'); // Works!
res.send('Not Found').status(404); // Does not work.
```
