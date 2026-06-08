

# Control Flow

Direct which path the program takes.

## if / else

```typescript
const age = 20

if (age >= 18) {
  console.log("adult")
} else {
  console.log("minor")
}
```

Chain multiple conditions:

```typescript
const score = 85

if (score >= 90) {
  console.log("A")
} else if (score >= 80) {
  console.log("B")
} else if (score >= 70) {
  console.log("C")
} else {
  console.log("F")
}
```

Conditions are checked top to bottom. The first truthy condition wins. The `else` block catches everything else.

## Ternary operator

Short form for simple if/else:

```typescript
const status = age >= 18 ? "adult" : "minor"
```

Use for simple assignments. Use `if/else` for anything more complex.

## switch

Match a value against multiple cases:

```typescript
const method = "POST"

switch (method) {
  case "GET":
    handleGet()
    break
  case "POST":
    handlePost()
    break
  case "DELETE":
    handleDelete()
    break
  default:
    throw new Error(`Unsupported method: ${method}`)
}
```

Always include `break` (or `return`). Without it, execution falls through to the next case.

## for...of — iterate over values

```typescript
const fruits = ["apple", "banana", "cherry"]

for (const fruit of fruits) {
  console.log(fruit)
}
```

Use `for...of` for arrays. It gives you each element directly.

## for...in — iterate over keys

```typescript
const user = { name: "Ada", age: 25, email: "ada@example.com" }

for (const key in user) {
  console.log(`${key}: ${user[key as keyof typeof user]}`)
}
```

Use `for...in` for objects. It gives you each key.

## while — repeat until condition is false

```typescript
let retries = 0

while (retries < 3) {
  const connected = tryConnect()
  if (connected) break
  retries++
}
```

Use `while` when you don't know how many iterations you need.

## break and continue

`break` exits the loop entirely. `continue` skips to the next iteration.

```typescript
const numbers = [1, 2, 3, 4, 5, 6]

for (const n of numbers) {
  if (n === 3) continue  // skip 3
  if (n === 6) break     // stop at 6
  console.log(n)         // prints: 1, 2, 4, 5
}
```
