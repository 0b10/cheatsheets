# Server Side
## TODO

### Research

* Flow types
* Introspection
* Promises

## Misc

### Glossary 

* Fields: A member of a type/object. Typically is a scalar, or another type/object. They are not arguments.

<details>
	<summary>Perspective</summary>

---

#### Fields

Each field is essentially just a function:

```graphql
// User type
{
	name: String
}
```

Becomes:

```javascript
// User type
{
	name(user) { // Arg == parent field value
		return user.name;
	}
}
```

---

</details>

## Doc ([Spec](https://graphql.github.io/graphql-spec/June2018/#sec-Descriptions))

Documentation is placed before the entity being destribed, and makes use of markdown

```graphql
"""
Yadda yadda. Query eye for the REST guy.
"""
type Query {
	"""
	Documenting the obvious isn't necessary.
	"""
	foo(
		"""
		Even args, in this peculiar manner.
		"""
		bar: String
	) : String
}
```

## Root Operations ([Spec](https://graphql.github.io/graphql-spec/June2018/#sec-Root-Operation-Types))

* The root query type is mandatory, Subscription/Mutaiton types are not - they are optional. 
* The schema definition can be omitted if the root operation types use the default names: 'Query', 'Mutation', and 'Subscription'.


## Tips
* Schemas can be [extended](https://graphql.github.io/graphql-spec/June2018/#sec-Schema-Extension)

## Types ([Spec](https://graphql.github.io/graphql-spec/June2018/#sec-Types))

### Named Types

There are 6 different fundamental types: **scalar**, **object**, **interface**, **union**, and **input**.

These are called **'named types'**, and can be wrapped by **'wrapping types'**.

#### Scalar ([Spec](https://graphql.github.io/graphql-spec/June2018/#sec-Scalars))

* A primitive value: Int (signed), Float, String, Boolean, ID
* **New scalars** can be **defined** - e.g. Time, which could serialise to a string that conforms to ISO 8601
* Forms the leaves of the graph
* **Enumerable** - see Enum type
* Input and output coercion mean that types can be used in place of other types
  * e.g. 1 for false, and 0 for true, or 1 accepted as a float
  * Must be within the confines of coercion rules - e.g. see [int](https://graphql.github.io/graphql-spec/June2018/#sec-Scalars) coercion spec.
* Can be nullable
  * They can be null, or the type specified
  * Unless given a bang!
* Representable as String in all cases
* Can be [extended](https://graphql.github.io/graphql-spec/June2018/#ScalarTypeExtension)

<details>
	<summary>Examples</summary>

---

Defining a scalar type for *time*:

```graphql
scalar Time
```

---

</details>

#### Object

* Defines a set of fields
* Each field can be any type - possibly forming hierarchies

#### Interface

* Defined as an abstract type
* Works similar to OOP interfaces
  * Defines a list of fields - objects that implement the interface are guarenteed to implement the fields
  * A type system can return an inteface
  
#### Union

* Similar to interfaces, except it defines multiple types - i.e. it's a union of types
* A type system that claims to return a union, means that it will return one of those types

#### Input

* Defines the expected arguments for input fields

<details>
	<summary>Example</summary>

---

```graphql
input SomeInput {
	title: String!
	description: String!
}
```

This type can be used within the schema to define input:

```graphql
type Mutation {
	doStuff(argName: SomeInput): String
}
```

Now doStuff will expected a 'title' and a 'description'.

Note, that these fields are encapsulated within a type, and when accessing them within the resolver, you have to consider its namespace: `args.argName.title`

---

</details>

---

### Input/Output types

This is more of a category of types that can be used for input an output

* **Scalar** and **Enum** types can be used for both input and output.
* **Lists** and **Non-null** can be used for both, depending on the types they wrap.
* The **Input** type can be used only for input
* **Object**, **Enum**, and **Union** types can only be used as *output* types.

---

### Wrapping types

* Lists: self explanatory
* Non-null: defines a type can never be null, not even on error

These *wrap* named types, and are continually unwrapped until named types are found - e.g. when used as input, or output types.

# Client Side
## Fragments

* Essentially these are reusable functions
* They allow complex queries to be built up with 'fragments', small subqueries
