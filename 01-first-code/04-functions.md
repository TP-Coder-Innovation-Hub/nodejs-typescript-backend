[Entry]

# Functions

Functions are reusable blocks of code. You define them once, call them anywhere.

## Arrow functions

The default way to write functions in modern TypeScript:

```typescript
const add = (a: number, b: number): number => {
  return a + b
}

add(2, 3) // 5
```

Short form for single expressions:

```typescript
const double = (n: number): number => n * 2
```

The `return` is implicit when you omit the curly braces.

Why arrow functions? Two reasons: shorter syntax, and lexical `this`. In traditional functions, `this` depends on how the function is called. Arrow functions inherit `this` from the surrounding scope. This matters in classes and callbacks.

## Parameters

**Default values:**
```typescript
const greet = (name: string, greeting = "Hello"): string => {
  return `${greeting}, ${name}`
}

greet("Ada")              // "Hello, Ada"
greet("Ada", "Welcome")  // "Welcome, Ada"
```

**Optional parameters:**
```typescript
const createUser = (name: string, email?: string): User => {
  return { name, email: email ?? "not provided" }
}
```

**Rest parameters:**
```typescript
const sum = (...numbers: number[]): number => {
  return numbers.reduce((total, n) => total + n, 0)
}

sum(1, 2, 3) // 6
```

## Closures

A function remembers the variables from where it was created:

```typescript
const createCounter = () => {
  let count = 0
  return {
    increment: () => ++count,
    getCount: () => count,
  }
}

const counter = createCounter()
counter.increment() // 1
counter.increment() // 2
counter.getCount()  // 2
```

`count` is private. Only the returned functions can access it. This is how you create encapsulation without classes.

## Functions as values

Functions can be stored in variables, passed as arguments, and returned from other functions:

```typescript
const apply = (fn: (n: number) => number, value: number): number => {
  return fn(value)
}

const double = (n: number) => n * 2
const square = (n: number) => n * n

apply(double, 5)  // 10
apply(square, 5)  // 25
```

This is functional programming in practice. You will use this pattern constantly with array methods like `map`, `filter`, and `reduce`.
