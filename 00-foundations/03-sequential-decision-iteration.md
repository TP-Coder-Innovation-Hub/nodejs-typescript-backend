

# Sequential, Decision, Iteration

Every program, no matter how complex, uses three building blocks. That is it.

## 1. Sequential — do this, then this, then this

Code runs top to bottom, one line after another.

```typescript
const name = "Ada"
const greeting = `Hello, ${name}`
console.log(greeting)
```

This is the default. Most of your code is sequential.

## 2. Decision — do different things based on a condition

```typescript
const age = 17

if (age >= 18) {
  console.log("Allowed")
} else {
  console.log("Too young")
}
```

The program checks a condition (`age >= 18`) and picks a path. Binary choice. You can chain multiple conditions:

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

For checking specific values, use `switch`:

```typescript
const method = "POST"

switch (method) {
  case "GET":
    handleGet()
    break
  case "POST":
    handlePost()
    break
  default:
    handleUnknown()
}
```

## 3. Iteration — repeat something

```typescript
const names = ["Ada", "Grace", "Linus"]

for (const name of names) {
  console.log(name)
}
```

Loop over collections. Do something with each item. Stop when done.

`for...of` iterates over values. `for...in` iterates over keys. `while` repeats until a condition is false.

```typescript
// while: keep going until condition is false
let attempts = 0
while (attempts < 3) {
  const success = tryConnect()
  if (success) break
  attempts++
}
```

## These three combine into everything

```typescript
const users = [
  { name: "Ada", age: 25 },
  { name: "Grace", age: 17 },
  { name: "Linus", age: 30 },
]

// Sequential: declare the result
const adults: string[] = []

// Iteration: loop through users
for (const user of users) {
  // Decision: filter by age
  if (user.age >= 18) {
    adults.push(user.name)
  }
}

// Sequential: output
console.log(adults) // ["Ada", "Linus"]
```

Sequential. Decision. Iteration. Everything else is just shortcuts for these three.
