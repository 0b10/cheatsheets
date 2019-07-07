# IntersectionalObserver

## Summary

Fire an event when two elements intersect on screen.

The two key elements involved:

* root
  * The relative element, or viewport.
  * This could be the main content div in a web page, or the document's viewport.
* target
  * The observed object that will intersect
  * This could be an element at the foot of a web page, that triggers an action when it intersects the root (viewport, other element etc.)


## Observer Entry
* An observer entry is a state at a sinlge point of time, and it contains objects that wrap the root, and target, to provide useful information.
* The a callback attached to the observer will be sent an **array** of entry objects (IntersectionObserverEntry) as the first argument.
* For each entry item, the following interface is available:

[IntersectionObserverEntry](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry) 
  * Represents a single state, at a single point of time, for either the root or the target
  * Has the following interface (key behaviours only):
    [boundingClientRect](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry/boundingClientRect) 
      * Returns the target and it's state (for a single point in time)
      * The return value is an object describing the target's rectangle
      * Returned type: [DOMRectReadOnly](https://developer.mozilla.org/en-US/docs/Web/API/DOMRectReadOnly)
      * properties:
        * x, y, top, bottom, left, right, width, height
        * x, y is the origin - top left
    [rootBounds](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry/rootBounds)
      * Returns the root and it's state (for a single point in time)
      * The return value is an object describing the root's intersection rectangle
      * Returned type: [DOMRectReadOnly](https://developer.mozilla.org/en-US/docs/Web/API/DOMRectReadOnly)
      * properties:
        * x, y, top, bottom, left, right, width, height
        * x, y is the origin - top left
    [intersectionRatio](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry/intersectionRatio)
      * Determines how much of an intersection between the two elements has been made
      * The percentage of intersection as a ratio - 0.0 -> 1.0
      * Use this to determine direction - an increase means a greater degree of intersection (e.g. visibility)
    [isIntersecting](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry/isIntersecting)
      * A 
    target
      * This is the observed element
      * But this only points to an element that has experienced a change, relative to the root.


## Create The observer 

This is the example given, that checks against each interSection ratio in the list of entries, to determine the direction
of scrolling. This might need a number of thresholds to be set on the target, so that a number of events are fired, and can
be measured.

```js
function callback(entries, observer) {
  // Entries is an array
  entries.forEach(entry => {
    if (entry.isIntersecting) { // Only if
      // Each entry has a target, root, and some other properties
      const target = entry.boundingClientRect();
      const root = entry.rootBounds();
      const current = entry.intersectionRatio

      if(current > prev) {
        // more visible
      } else if(current < prev) {
        // less visible
      } else {
        // no change
      }
      
      let prev = current;
    }
    
  })
}

const observer = new IntersectionObserver(callback, {...options});
observer.observe(anElement);
```

## A Simple Example

This example is more simple, and relies on the fact that entries is of length 1. I have observed this with an actual implementation, where
a button is being observed. When the button appears into the viewport (or root), entry.isIntersecting is true, and false if it disappears out of the
viewport.

```js
function callback(entries) {
  if (entries.length > 1) throw Error(); // > 1 is probably bad
  const intersecting = entries[0].isIntersecting;
  if (intersecting) doStuff();
}

const observer = new IntersectionObserver(callback); // No options.root means teh viewport is the root
observer.observe(anElement);
```

## Issues

* If the target intersects the viewport, but your action moves it out of the viewport, the callback will be called twice. To fix this, you have to define specific rules within the callback - e.g. an infinite scroll should only load posts when scrolling in one direction.i See examples above.



