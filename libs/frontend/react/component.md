## Misc

- `forceUpdate()` ignores `shouldComponentUpdate()`
- changes to state or props will cause a re-render

## Classes

* Statefull (functions are not)

### Properties [Docs](https://reactjs.org/docs/react-component.html#class-properties-1)
* **defaultProps** is a class (static?) property, and is used to define undefined props - but does not define props that are null.
* **displayName** is for debugging, by defualt it's the function name.
* **props** yes, this is a class property


## setState()


- **Enqueues** state update requests, which are processed as batches. This is a request, and changes are not necessarily applied immediately.
- States are **batched**, and the objects are **(shallow) merged**. Subsequent objects overwrite if there are name clashes on the properties being targetted.
- Avoid unnecessary `setState()` calls as they initiate re-rendering. Use conditions, or `shouldComponentUpdate()`

### Callbacks

`setState(one, two)` provides two callbacks:

* 'one'.. for updating the state, with a guarantee that the state and props are in a good state before doing so
* 'two'.. for executing some function after the state has been updated - an alternative to `componentDidUpdate()`, the latter being the recommended approach

#### Callback One

This callback should be **used when** the new state depends on the old state/props in some way:

```
(state, props) => {state object}
```

* Should return a state object
* 'state' and 'props' are references
* 'state' and 'props' are guarenteed to be up-to-date.
* The output is shallow merged with state

```jsx
this.setState((state, props) => {
	return { foo: state.foo + 1, bar: props.bar };
})
```


#### Callback Two

This is optional, and executes after the state has been updated.

## Lifecyle

### Cheatsheet

![React Lifecycle Cheatsheet](https://cdn-images-1.medium.com/max/3668/1*gMdgkSygxwy9mlJCFyphBg.png)

### Init

These execute in the following order when **initialising** a component

1. **constructor()**
	* should be **pure**
	* set state directly: `this.state = {}`, don't clal setState() (re-renders)
		- Don't do this `this.state = { foo: this.props.foo }
			* updates to 'props.foo' won't be reflected in 'foo', because this is performed once, and not repeatedly.
			* Use props directly, why not? it's there.
	* Use for initialising state and binding methods
1. static **getDerivedStateFromProps()**
	* Updates internal state when there's a change in props
	* Use sparingly
	* Don't update state unconditionally, or just because state and props don't match: [source](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)
1. **render()**
	* should be **pure**, avoid setting state in here
1. **componentDidMount()**
	* runs after rendered components have been set to the DOM 
	* good for:
		- **subscriptions**
		- **side-effects** - like XHR
		- initialisation that requires a DOM node
	* **setState()** *can* be called here, but it should be called *immediately*, and it causes re-rendering. This means the component is rendered twice, before it's reflected in the DOM. Use this careflly, as it can cause performance issues
	* 

### Update

Updates occur on setState and updates to props

This is the order when updating (re-rendering) a component:

1. static **getDerivedStateFromProps()**
	* Rarely used 
	* Updates the state
	* Called before render()
	* It should return a state object, or null to not update
	* Used in cases where state changes over time - e.g. a transition effect
	* static, doesn't have access to the instance
1. **shouldComponentUpdate()**: [Docs](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)
	* Rarely used
	* Read the docs for caveats
	* should return **true** or **false**
	* If true, render() isn't called
	* Not called for the initial render, or when `forceUpdate()
	* Use PureComponent to prevent rendering, use this function just for performance optimisations.
	* Does a shallow comparison of props and state
1. **render()**
	* re-renders all children before rendering self
	* Must be pure, it must not mutate state, or modify the DOM. Use `componentDidMount()` for the latter.
	* Can return multiple types, like arrays, but typically a react element. [docs](https://reactjs.org/docs/react-component.html#render)
1. **getSnapshotBeforeUpdate()**
	* Rarely used 
	* Captures information before it's rendered in the DOM.
	* An example is a chat program handling scroll positions in a special way, before rendering
1. **componentDidUpdate()** : [Docs](https://reactjs.org/docs/react-component.html#componentdidupdate)
	* Called after an update, but not after the initial render
	* Good for
		- Working on DOM nodes after an update
		- Network requests, if necessary - e.g. if state has changed
	* setState() can be called **immediately**, but *must* be wrapped int a **condition**, otherwise you may cause an infinite loop - the condition must only allow the execition of setState the desired number of times, i.e. once. So, when the state has changed, the expression will evaluate differently.

### Destroy

* This is called before removing the element from the DOM: `componentWillUnmount()`
* Examples
	* **Invalidating timers**: clearInterval(), which cancels a timeout.
	* Cancelling **network requests**
	* Removing **subscriptions**

### Errors [(Docs)](https://reactjs.org/docs/react-component.html#error-boundaries)

* These are React components (ErrorBoundary) that catch errors anywhere in their child subtree, but not itself.
* They can be used to update the state while recovering from an error, instead of crashing.
* Returns a state value: { foo: "bar" }
* To implement an ErrorBoundary component you just need to implement one of the following:
	* static **getDerivedStateFromError()**
		- Called during a render() error, so no side-effects are permitted
		- Return desired state
	* **componentDidCatch()**
		- Called during the commit phase
		- Side effects allowed
		- Use for logging


## Glossary

* Mount: a component has mounted after it has been rendered to the DOM - i.e. 'mounted on the DOM'