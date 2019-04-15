# All Versions
## Object Literal vs. JSON
* They are not the same thing.
* JSON requires property names to be strings: `"property":...`
* JSON values do not accept named references, their values are literal. Object literals can hold variables/functions etc.

# ES6
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

## Shorthand
#### Object Literals

```javascript
const baz = 'baz';
const foo = {
  bar: 'bar',
  baz, // No need for baz: baz,
}
```
