# Appendix A: Specification of FieldSelectionMap Scalar

## Introduction

This appendix focuses on the specification of the {FieldSelectionMap} scalar type. {FieldSelectionMap} is
designed to express semantic equivalence between arguments of a field and fields within the result 
type. Specifically, it allows defining complex relationships between input arguments and fields in
the output object by encapsulating these relationships within a parsable string format. It is used
in the `@is` and `@requires` directives.

To illustrate, consider a simple example from a GraphQL schema:

```graphql
type Query {
    userById(userId: ID! @is(field: "id")): User! @lookup
}
```

In this schema, the `userById` query uses the `@is` directive with {FieldSelectionMap} to declare that
the `userId` argument is semantically equivalent to the `User.id` field. 

An example query might look like this:

```graphql
query {
    userById(userId: "123") {
        id
    }
}
```

Here, it is exptected that the `userId` "123" corresponds directly to `User.id`, resulting in the
following response if correctly implemented:

```json
{
    "data": {
        "userById": {
            "id": "123"
        }
    }
}
```

The {FieldSelectionMap} scalar type is used to establish semantic equivalence between an argument and
the fields within the associated return type. To accomplish this, the scalar must define the
relationship between input fields or arguments and the fields in the resulting object. 
Consequently, a {FieldSelectionMap} only makes sense in the context of a specific argument and its 
return type.

The {FieldSelectionMap} scalar is represented as a string that, when parsed, produces a {SelectedValue}.

A {SelectedValue} must exactly match the shape of the argument value to be considered
valid. For non-scalar arguments, you must specify each field of the input type in
{SelectedObjectValue}.

```graphql example
extend type Query {
    findUserByName(user: UserInput! @is(field: "{ firstName: firstName }")): User @lookup
}
```

```graphql counter-example
extend type Query {
    findUserByName(user: UserInput! @is(field: "firstName")): User @lookup
}
```


## Language

According to the GraphQL specification, an argument is a key-value pair in which the key is the name
of the argument and the value is a `Value`.

The `Value` of an argument can take various forms: it might be a scalar value (such as `Int`,
`Float`, `String`, `Boolean`, `Null`, or `Enum`), a list (`ListValue`), an input object
(`ObjectValue`), or a `Variable`.

Within the scope of the {FieldSelectionMap}, the relationship between input and output is
established by defining the `Value` of the argument as a selection of fields from the output object.

Yet only certain types of `Value` have a semantic meaning. 
`ObjectValue` and `ListValue` are used to define the structure of the value. 
Scalar values, on the other hand, do not carry semantic importance in this context, and variables
are excluded as they do not exist. 
Given that these potential values do not align with the standard literals defined in the GraphQL
specification, a new literal called {SelectedValue} is introduced, along with {SelectedObjectValue},

Beyond these literals, an additional literal called {Path} is necessary.

### Name

Is equivalent to the {Name} defined in the 
[GraphQL specification](https://spec.graphql.org/October2021/#Name)

### Path
Path :: 
    - FieldName
    - Path . FieldName
    - FieldName < TypeName > . Path

FieldName ::
    - Name

TypeName ::
    - Name

The {Path} literal is a string used to select a single output value from the _return type_ by
specifying a path to that value. 
This path is defined as a sequence of field names, each separated by a period (`.`) to create 
segments. 

``` example
book.title
```

Each segment specifies a field in the context of the parent, with the root segment referencing a
field in the _return type_ of the query. 
Arguments are not allowed in a {Path}.

To select a field when dealing with abstract types, the segment selecting the parent field must 
specify the concrete type of the field using angle brackets after the field name if the field is not 
defined on an interface.

In the following example, the path `mediaById.<Book>.isbn` specifies that `mediaById` returns a
`Book`, and the `isbn` field is selected from that `Book`.

``` example
mediaById<Book>.isbn
```

### SelectedValue
SelectedValue :: 
    - Path
    - SelectedObjectValue
    - Path . SelectedObjectValue
    - SelectedValue | SelectedValue 

A {SelectedValue} is defined as either a {Path} or a {SelectedObjectValue}

A {Path} is designed to point to only a single value, although it may reference multiple fields
depending on the return type. To allow selection from different paths based on
type, a {Path} can include multiple paths separated by a pipe (`|`).

In the following example, the value could be `title` when referring to a `Book` and `movieTitle`
when referring to a `Movie`. 

``` example
mediaById<Book>.title | mediaById<Movie>.movieTitle
```

The `|` operator can be used to match multiple possible {SelectedValue}. This operator is applied
when mapping an abstract output type to a `@oneOf` input type.

```example
{ movieId: <Movie>.id } | { productId: <Product>.id }
```

```example
{ nested: { movieId: <Movie>.id } | { productId: <Product>.id }}
```

To select nested structured data, a {SelectedObjectValue} can be used as a segment in the path. The value
must select data in the same shape as the input. This is primarily used when mapping an object
inside a list to an input. The fields within a {SelectedObjectValue} are scoped to the parent field
of the path.

```graphql example
type Query {
    findLocation(location: LocationInput! @is(field: "{ coordinates: coordinates.{ lat: x, lon: y}}")): Location @lookup
}

type Coordinate {
    x: Int!
    y: Int!
}

type Location {
    coordinates: [Coordinate!]!
}

input PositionInput {
    lat: Int!
    lon: Int!
}

input LocationInput {
    coordinates: [PositionInput!]!
}
```

### SelectedObjectValue
SelectedObjectValue :: 
    - { SelectedObjectField+ }

SelectedObjectField :: 
    - Name: SelectedValue

{SelectedObjectValue} are unordered lists of keyed input values wrapped in curly-braces `{}`. 
This structure is similar to the `ObjectValue` defined in the GraphQL specification, but it
differs by allowing the inclusion of {Path} values within a {SelectedValue}, thus extending the
traditional `ObjectValue` capabilities to support direct path selections.

### Name
Is equivalent to the `Name` defined in the GraphQL specification.

## Validation
Validation ensures that {FieldSelectionMap} scalars are semantically correct within the given context. 

Validation of {FieldSelectionMap} scalars occurs during the composition phase, ensuring that all
{FieldSelectionMap} entries are syntactically correct and semantically meaningful relative to the
context. 

Composition is only possible if the {FieldSelectionMap} is validated successfully. An invalid
{FieldSelectionMap} results in undefined behavior, making composition impossible.

### Examples
In this section, we will assume the following type system in order to demonstrate examples:

```graphql
type Query {
    mediaById(mediaId: ID!): Media 
    findMedia(input: FindMediaInput): Media 
    searchStore(search: SearchStoreInput): [Store]!
    storeById(id: ID!): Store
}

type Store {
    id: ID!
    city: String!
    media: [Media!]!
}

interface Media {
    id: ID!
}

type Book implements Media {
    id: ID!
    title: String!
    isbn: String!
    author: Author!
}

type Movie implements Media {
    id: ID!
    movieTitle: String!
    releaseDate: String!
}

type Author {
    id: ID!
    books: [Book!]!
}

input FindMediaInput @oneOf {
    bookId: ID
    movieId: ID
}

type SearchStoreInput {
    city: String
    hasInStock: FindMediaInput
}
```

### Path Field Selections

Each segment of a {Path} must correspond to a valid field defined on the current type context.

**Formal Specification**

-  For each {segment} in the {Path}:
  - If the {segment} is a field
    - Let {fieldName} be the field name in the current {segment}.
    - {fieldName} must be defined on the current type in scope.

**Explanatory Text**

The {Path} literal is used to reference a specific output field from a input field.
Each segment in the {Path} must correspond to a field that is valid within the current type scope.

For example, the following {Path} is valid in the context of `Book`:

```graphql example
title
```

```graphql example
<Book>.title
```

Incorrect paths where the field does not exist on the specified type is not valid result in
validation errors. For instance, if `<Book>.movieId` is referenced but `movieId` is not a field of `Book`,
will result in an invalid {Path}.

```graphql counter-example
movieId
```

```graphql counter-example
<Book>.movieId
```

### Path Terminal Field Selections

Each terminal segment of a {Path} must follow the rules regarding whether the selected field is a
leaf node.

**Formal Specification**

-  For each {segment} in the {Path}:
  - Let {selectedType} be the unwrapped type of the current {segment}.
  - If {selectedType} is a scalar or enum:
    - There must not be any further segments in {Path}.
  - If {selectedType} is an object, interface, or union:
    - There must be another segment in {Path}.

**Explanatory Text**

A {Path} that refers to scalar or enum fields must end at those fields. No further field selections
are allowed after a scalar or enum. On the other hand, fields returning objects, interfaces, or
unions must continue to specify further selections until you reach a scalar or enum field.

For example, the following {Path} is valid if `title` is a scalar field on the `Book` type:

```graphql example
book.title
```

The following {Path} is invalid because `title` should not have subselections:

```graphql counter-example
book.title.something
```

For non-leaf fields, the {Path} must continue to specify subselections until a leaf field is reached:

```graphql example
book.author.id
```

Invalid {Path} where non-leaf fields do not have further selections:

```graphql counter-example
book.author
```

### Type Reference Is Possible

Each segment of a {Path} that references a type, must be a type that is valid in the current
context.

**Formal Specification**

-  For each {segment} in a {Path}:
  - If {segment} is a type reference:
    - Let {type} be the type referenced in the {segment}.
    - Let {parentType} be the type of the parent of the {segment}.
    - Let {applicableTypes} be the intersection of
            {GetPossibleTypes(type)} and {GetPossibleTypes(parentType)}.
    - {applicableTypes} must not be empty.

GetPossibleTypes(type):

-  If {type} is an object type, return a set containing {type}.
-  If {type} is an interface type, return the set of types implementing {type}.
-  If {type} is a union type, return the set of possible types of {type}.

**Explanatory Text**

Type references inside a {Path} must be valid within the context of the surrounding type. A type
reference is only valid if the referenced type could logically apply within the parent type.


### Values of Correct Type

**Formal Specification**

-  For each SelectedValue {value}:
  - Let {type} be the type expected in the position {value} is found.
  - {value} must be coercible to {type}.

**Explanatory Text**

Literal values must be compatible with the type expected in the position they are found.

The following examples are valid use of value literals in the context of {FieldSelectionMap} scalar:

```graphql example
type Query {
  storeById(id: ID! @is(field: "id")): Store! @lookup
}

type Store {
    id: ID
    city: String!
}
```

Non-coercible values are invalid. The following examples are invalid:

```graphql counter-example
type Query {
  storeById(id: ID! @is(field: "id")): Store! @lookup
}

type Store {
    id: Int
    city: String!
}
```

### Selected Object Field Names

**Formal Specification**

-  For each Selected Object Field {field} in the document:
  - Let {fieldName} be the Name of {field}.
  - Let {fieldDefinition} be the field definition provided by the parent selected object type named {fieldName}.
  - {fieldDefinition} must exist.

**Explanatory Text**

Every field provided in an selected object value must be defined in the set of possible fields of that input object's expected type.

For example, the following is valid:

```graphql example
type Query {
  storeById(id: ID! @is(field: "id")): Store! @lookup
}

type Store {
    id: ID
    city: String!
}
```

In contrast, the following is invalid because it uses a field "address" which is not defined on the expected type:

```graphql counter-example
extend type Query {
  storeById(id: ID! @is(field: "address")): Store! @lookup
}

type Store {
    id: ID
    city: String!
}
```

### Selected Object Field Uniqueness

**Formal Specification**

-   For each selected object value {selectedObject}:
  - For every {field} in {selectedObject}:
    - Let {name} be the Name of {field}.
    - Let {fields} be all Selected Object Fields named {name} in {selectedObject}.
    - {fields} must be the set containing only {field}.

**Explanatory Text**

Selected objects must not contain more than one field with the same name, as it would create ambiguity and potential conflicts.

For example, the following is invalid:

```graphql counter-example
extend type Query {
  storeById(id: ID! @is(field: "id id")): Store! @lookup
}

type Store {
    id: ID
    city: String!
}
```

### Required Selected Object Fields

**Formal Specification**

-   For each Selected Object:
  - Let {fields} be the fields provided by that Selected Object.
  - Let {fieldDefinitions} be the set of input object field definitions of that Selected Object.
  - For each {fieldDefinition} in {fieldDefinitions}:
    - Let {type} be the expected type of {fieldDefinition}.
    - Let {defaultValue} be the default value of {fieldDefinition}.
    - If {type} is Non-Null and {defaultValue} does not exist:
      - Let {fieldName} be the name of {fieldDefinition}.
      - Let {field} be the input object field in {fields} named {fieldName}.
      - {field} must exist.

**Explanatory Text**

Input object fields may be required. This means that a selected object field is required if the corresponding input field is required. Otherwise, the selected object field is optional.

For instance, if the `UserInput` type requires the `id` field:

```graphql example
input UserInput {
    id: ID!
    name: String!
}
```

Then, an invalid selection would be missing the required `id` field:

```graphql counter-example
extend type Query {
  userById(user: UserInput! @is(field: "{ name: name }")): User! @lookup
}
```

If the `UserInput` type requires the `name` field, but the `User` type has an optional `name` field, the following selection would be valid.

```graphql example
extend type Query {
  findUser(input: UserInput! @is(field: "{ name: name }")): User! @lookup
}

type User {
    id: ID
    name: String
}

input UserInput {
    id: ID
    name: String!
}
```

But if the `UserInput` type requires the `name` field but it's not defined in the `User` type, the selection would be invalid.

```graphql counter-example
extend type Query {
  findUser(input: UserInput! @is(field: "{ id: id }")): User! @lookup
}

type User {
    id: ID
}

input UserInput {
    id: ID
    name: String!
}
```