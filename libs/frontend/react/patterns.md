# Mixed Component
* State, side effects, and views all in one component.
* Side effects through calling methods to return a view.
* Side effecrs quickly become difficult to understand.

# Render prop

```jsx
// Define
<MyComponent render={() => <div />} />

// ...
// Call as prop
render() { return this.props.render() }

```

A more complex example


```jsx
// Define, accept args
<MyComponent render={props => <div {...props} />} />

// ...
// Call as prop, give args
render() { return this.props.render({ a: "a" }) }

```

You can use children


```jsx
// Define, and accept children. Notice the callback?
<MyComponent>
  {props => <div {...props} />}
</MyComponent>

// ...
// Call as props.children(), give args
render() { return this.props.children({ a: "a" }) }

```

# HOC

Basically a closure. This example does nothing, it just forwards props.

```jsx
const withNothing = WrappedComponent => {
  return props => (
    WrappedComponent(props)
  )
}

const Foo = withNothing(MyComponent)({ a: "a" })
```

# Compose

This is more of a functional pattern

```js
// Make f(g(h()))
compose(f, g, h)
pipe(h, g, f) // Same principle, but backwards
```

Perhaps each function could be a HOC:

```jsx
compose(
  f(Foo),
  g(Bar),
  h(Baz)
)("arg")
```

What this means is compose() will produce a product component, and that new component shall be called with "arg".
These arguments are entirely subjective, and specific to your own implementation. Perhaps the HOCS inject props, and arg is
another prop to produce the final component.

