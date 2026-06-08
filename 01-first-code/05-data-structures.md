[Entry]

# Data Structures

You need to store and organize data. These are the built-in options.

## Array — ordered list

Use when you need a sequence of items, accessed by index.

```typescript
const fruits: string[] = ["apple", "banana", "cherry"]

fruits[0]                    // "apple"
fruits.push("date")         // add to end
fruits.pop()                // remove from end
fruits.includes("banana")   // true
fruits.length               // 4
```

Common operations:

```typescript
const numbers = [1, 2, 3, 4, 5]

// Transform each element
const doubled = numbers.map(n => n * 2)          // [2, 4, 6, 8, 10]

// Filter elements
const evens = numbers.filter(n => n % 2 === 0)   // [2, 4]

// Accumulate into a single value
const total = numbers.reduce((sum, n) => sum + n, 0) // 15

// Find a single element
const found = numbers.find(n => n > 3)           // 4

// Check a condition
const hasEven = numbers.some(n => n % 2 === 0)   // true
const allPositive = numbers.every(n => n > 0)     // true
```

`map`, `filter`, and `reduce` are the most used. Learn them well.

## Object — key-value pairs

Use for structured data with named fields.

```typescript
const user = {
  name: "Ada",
  age: 25,
  email: "ada@example.com",
}

user.name              // "Ada"
user["name"]           // "Ada" (same thing, bracket syntax)
user.role = "admin"    // add a new field
delete user.email      // remove a field
```

With TypeScript types:

```typescript
type User = {
  name: string
  age: number
  email: string
}

const user: User = { name: "Ada", age: 25, email: "ada@example.com" }
```

Destructuring:

```typescript
const { name, age } = user   // extract fields into variables
```

Spread to create new objects:

```typescript
const updated = { ...user, age: 26 }  // copy all fields, override age
```

## Map — key-value with any key type

Use when you need keys that are not strings, or when you need to iterate in insertion order.

```typescript
const scores = new Map<string, number>()

scores.set("Ada", 95)
scores.set("Grace", 88)
scores.get("Ada")         // 95
scores.has("Linus")       // false
scores.delete("Grace")    // true
scores.size               // 1
```

Why Map over Object? Map preserves insertion order, has a `size` property, and accepts any type as a key. Object keys are always strings (or symbols).

## Set — unique values

Use when you need to eliminate duplicates or check membership.

```typescript
const tags = new Set<string>()

tags.add("typescript")
tags.add("backend")
tags.add("typescript")  // ignored, already exists
tags.has("backend")     // true
tags.size               // 2
```

Remove duplicates from an array:

```typescript
const unique = [...new Set([1, 2, 2, 3, 3, 3])] // [1, 2, 3]
```

## When to use which

| Structure | Use when |
|-----------|----------|
| Array | Ordered list, iteration, positional access |
| Object | Structured data with known shape |
| Map | Dynamic key-value pairs, non-string keys |
| Set | Unique values, membership checks |
