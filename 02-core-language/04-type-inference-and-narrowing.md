[Mid]

# Type Inference and Narrowing

You write fewer type annotations than you think. TypeScript infers types from context and narrows types based on checks.

## Type inference

TypeScript infers the type from the initial value:

```typescript
const name = "Ada"       // string
const age = 25           // number
const active = true      // boolean
const items = [1, 2, 3]  // number[]
```

No annotation needed. TypeScript knows.

Function return types are also inferred:

```typescript
const add = (a: number, b: number) => a + b  // return type inferred as number
```

You only need to annotate function parameters. TypeScript cannot infer those.

## When to write annotations

- Function parameters (required)
- Return types on public/exported functions (documentation)
- Variables where inference is too wide:
  ```typescript
  let status = "active"  // inferred as string, not "active"
  ```
  Fix with a const assertion or explicit type:
  ```typescript
  let status: "active" | "inactive" = "active"
  let status = "active" as const  // type is literally "active"
  ```

## Type narrowing

TypeScript narrows types based on runtime checks. You check a type, TypeScript knows inside the check:

### typeof

```typescript
function process(value: string | number) {
  if (typeof value === "string") {
    // TypeScript knows: value is string
    return value.toUpperCase()
  }
  // TypeScript knows: value is number
  return value * 2
}
```

### instanceof

```typescript
function handleError(error: unknown) {
  if (error instanceof Error) {
    // TypeScript knows: error is Error
    console.log(error.message)
  } else {
    console.log(String(error))
  }
}
```

### Truthiness check

```typescript
function getName(name: string | null) {
  if (name) {
    // TypeScript knows: name is string (not null)
    return name.toUpperCase()
  }
  return "ANONYMOUS"
}
```

### Equality check

```typescript
interface Success {
  status: "success"
  data: string
}

interface Failure {
  status: "failure"
  error: string
}

type Result = Success | Failure

function handleResult(result: Result) {
  if (result.status === "success") {
    // TypeScript knows: result is Success
    console.log(result.data)
  } else {
    // TypeScript knows: result is Failure
    console.log(result.error)
  }
}
```

This pattern — unions with a discriminant field (like `status`) — is called a discriminated union. It is the most common way to model results in TypeScript.

## The practical rule

Let TypeScript infer. Write annotations for function parameters and public APIs. Use narrowing instead of type casts. If you are writing `as` or `<Type>` casts frequently, something is wrong with your types.
