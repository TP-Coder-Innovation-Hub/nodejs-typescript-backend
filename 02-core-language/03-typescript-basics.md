

# TypeScript Basics

TypeScript is a type system on top of JavaScript. It catches errors before you run code.

## Why types

JavaScript lets you do this:

```javascript
function add(a, b) {
  return a + b
}

add("2", "3") // "23" — string concatenation, not addition
```

No error. No warning. Just wrong behavior. TypeScript prevents this:

```typescript
function add(a: number, b: number): number {
  return a + b
}

add("2", "3") // error: Argument of type 'string' is not assignable to parameter of type 'number'
```

The error is caught at compile time, before the code ever runs. This is the value: catch bugs early, not in production.

## Type annotations

```typescript
const name: string = "Ada"
const age: number = 25
const active: boolean = true
```

You can annotate, but TypeScript often infers the type:

```typescript
const name = "Ada"  // inferred as string
```

## Interfaces

Define the shape of an object:

```typescript
interface User {
  id: number
  name: string
  email: string
  age?: number  // optional
}

const user: User = {
  id: 1,
  name: "Ada",
  email: "ada@example.com",
}
```

`interface` and `type` are similar. Use `interface` for object shapes. Use `type` for unions, intersections, and complex types.

## Generics

Write code that works with multiple types while staying type-safe:

```typescript
function first<T>(items: T[]): T | undefined {
  return items[0]
}

first([1, 2, 3])         // number
first(["a", "b", "c"])   // string
first([])                // undefined
```

`T` is a type parameter. When you call `first([1, 2, 3])`, TypeScript infers `T = number`. The return type becomes `number | undefined`.

Generics appear everywhere in libraries. Elysia uses them for type-safe routes. Drizzle uses them for type-safe queries.

```typescript
// generic interface
interface ApiResponse<T> {
  data: T
  status: number
  message: string
}

const response: ApiResponse<User> = {
  data: { id: 1, name: "Ada", email: "ada@example.com" },
  status: 200,
  message: "ok",
}
```

## TypeScript is a layer

> 🖼️ **[IMAGE_PLACEHOLDER]** — TypeScript compiler pipeline ts to js type check erased

TypeScript compiles to JavaScript. The types are erased. At runtime, it is plain JavaScript. TypeScript does not change how your code runs. It changes how confident you are that your code is correct.

```
.ts file → TypeScript compiler → type check → .js file → runtime (Bun)
```
