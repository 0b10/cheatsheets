## Misc

* Component functions are stateless, they don't have `state = {}`, you need to use classes that extend Component for a state object.
* State updates are merged - e.g. one object into another. You can update in several locations.

## Anti-patterns [(Source)](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

* Unconditionally copying props to state
* Erasing state when props change

### A Single Source of Truth

* Don't mix controlled and uncontrolled patterns - don't partially control the state in or out of a component

### Asynchronous Calls and State [(Docs)](https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)

setState(), this.props, are asynchronous. Custom methods that setState() may also make use of other asynchronous calls - like setTimeout().

* Don't depend on these values to determine some other state.

* Don't `await setState()` because React batches these calls for perfomance.

The **correct way** to setState() when depending on a previous state is through a callback:

```jsx
this.setState((state, props) => ({
  foo: state.foo + props.bar
}));
```

State is the last known state, and props is the correct value at the time of updating.


An example of a custom method is a clock.

<details>

	This example will cause a re-render when setState is called, every second. As a result, componentDidMount() will be called after re-render - essentially these two processes feed each other.

</details>

setTimout() \[and setState()\] is \[are\] asynchronous, so depending on the result of this behaviour to set some other state is a bad idea.

```jsx
componentDidMount() {
	return setTimeout(() => this.setState({date: new Date()}), 1000); // Async
	console.log({date: this.state.date}, "Dont rely upon this");
}
```

```jsx
this.setState({foo: "foo"}); // Async
console.log(this.state.foo, "Or this")
```

## Lifecyle methods

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
	* runs after rendered components have been set on the DOM 
	* set **subscriptions** here
	* perform and **side-effects** (modify state) here

### Update

Updates occur on setState and updates to props

This is the order when updating (re-rendering) a component:

1. static **getDerivedStateFromProps()**
1. **shouldComponentUpdate()**
	* should return **true** or **false**
	* If true, render() isn't called
1. **render()**
	* re-renders all children before rendering self
	* Can return multiple types, like arrays, but typically a react element. [docs](https://reactjs.org/docs/react-component.html#render)
1. **getSnapshotBeforeUpdate()**
1. **componentDidUpdate()**

### Destroy

* This is called before removing the element from the DOM: `componentWillUnmount()`
* An example is clearInterval(), which cancels a timeout.

### Errors

* static **getDerivedStateFromError()**
* **componentDidCatch()**


## Glossary

* Mount: a component has mounted after it has been rendered to the DOM - i.e. 'mounted on the DOM'