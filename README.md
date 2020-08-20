# json-types
A type notation for JSON-based types.

## Goal
The goal of this document is to create a framework for specifying recursively defined types that are based on JSON. It should be easy to read and write and completely data based / function (so no classes here).

One can use this within runtime libraries, compilers and also just for specifying and talking about such types.

## Specification

### Type System
All types work just like in JSON. The following descriptions are a short reminder for the sake of common understanding.

#### Primitive Types
There are the following well known types:
- `Number` for 64 bit long floating point numbers
- `String` for any UTF-8 encoded String
- `null` which is actually only a value not directly addressable as type (has no Name). You should avoid `null` if possible.

#### composing types
There are types that combine types:
- `[]` is the array, an indexed ordered list
    - e.g. we write `[1,2,"hello"]` to combine two Strings and a String
- `{}` is the associative type (also called object or dictionary)
    - e.g. we write `{"a": 3, "b": "c"}` for a dictionary that contains the keys `a` and `b` and the respective values `3` and `"c"`

#### Any-Type
Like the `null`-type, you should not use the `Any` type. But since there are cases where you cannot do without...

### Type Descriptions
The syntax of our specification language is inspired by Typescript, Regular Expression Syntax and Haskell. We describe valid type descriptions and ways to derive valid type descriptions.

We write our rules in a semi-mathematical style. This should help with implementation and avoids ambiguity.

1. Values of primitive types are valid types, e.g. `3`, `"abc"`, `6.5`, `null`.
2. Primitive types are valid types i.e. `Number` and `String`.
3. If `T` and `U` are valid types, `T|U` is a valid type (either T or U)
4. If `T` is a valid type, 
    - `[T]` is a valid type (Array containing 1 Element of type `T`)
    - `[T*]` is a valid type (Array containing an arbitrary  amount of Elements of type `T`, including 0)
    - `[T+]` is a valid type (Array containing an arbitrary  amount of Elements of type `T`, excluding 0)
    - `[T?]` is a valid type (Array that optionally contains an element of type `T`)
    - `[T{n}]`is a valid type iff `n` is one or multiple comma-separated nonnegative integers
5. If `[X]` and `[Y]` are valid types, `[X,Y]` is a valid type  (`X` and `Y` can stand for anything).
6. If `T` is a valid type and `S` is a value of type `String`, then `{S:T}` is a valid type (contains key `S` and value `T`)
7. If `T` is a valid type and `S` is a value of type `String`, then `{S?:T}` is a valid type (optionally contains key `S` and value `T`)
8. If `{X}` and `{U:V}` are types (where `X` is anything, `U`, `V` are expressions that follow rule 6.) then `{X, U:V}` is a valid type
9. If `T` is a valid type, `(T)` is also a valid type

Note: `X` and `Y` in rule 5 can also be derived from this very rule (recursively).

### Type Notation
We define any type `T` with an expression `T: X` where `X` is either a valid *type description* as described in that chapter or another defined type. After introducing a type `T` we can use it in other type expressions like so `S: T`.

## Examples
Introducing the Null type (because we can and we are evil):
```
Null: null
```

Telephone contact book:
```
ContactList: [Contact*]
Contact: {
    "name": String,
    "number": String,
    "number_type": "private" | "buissness"
    "birthday": String
}
```






