[Entry]

# Variables and Types

Variables store values. Types describe what kind of values.

## Declaring variables

Use `const` by default. Use `let` only when you need to reassign.

```typescript
const name = "Ada"      // cannot be reassigned
let counter = 0          // can be reassigned
counter = counter + 1    // ok
// name = "Grace"        // error: cannot reassign const
```

Why `const` by default? Prevents accidental mutation. Most variables don't need to change. Make immutability the default, opt into mutation when needed.

Never use `var`. It has function scope instead of block scope, which causes bugs.

## Basic types

TypeScript has these primitive types:

```typescript
const text: string = "hello"
const num: number = 42
const yes: boolean = true
const empty: null = null
const missing: undefined = undefined
```

You rarely write type annotations explicitly. TypeScript infers the type from the value:

```typescript
const text = "hello"     // TypeScript knows this is string
const num = 42           // TypeScript knows this is number
```

## Dynamic typing under the hood

JavaScript is dynamically typed. A variable can hold any type:

```javascript
let value = "hello"
value = 42        // valid in JS, error in TS
```

TypeScript adds static typing on top. It checks types at compile time, then erases them. The runtime is still JavaScript.

## Arrays and objects

```typescript
const numbers: number[] = [1, 2, 3]
const names: string[] = ["Ada", "Grace"]

const user: { name: string; age: number } = {
  name: "Ada",
  age: 25,
}
```

## Type aliases

For complex types, create a named alias:

```typescript
type User = {
  name: string
  age: number
  email: string
}

const user: User = {
  name: "Ada",
  age: 25,
  email: "ada@example.com",
}
```

## Union types

A variable can be one of several types:

```typescript
type Status = "active" | "inactive" | "banned"

const status: Status = "active"
```

This is more useful than it looks. Union types with string literals let you define exact sets of valid values. The compiler checks that you only use valid ones.

## The rule

`const` by default. Let TypeScript infer types. Write annotations for function parameters and return types. The compiler handles the rest.
