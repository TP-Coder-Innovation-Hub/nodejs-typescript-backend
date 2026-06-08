[Entry]

# Programming Paradigms

A paradigm is a way of thinking about how code should be organized. JavaScript supports multiple paradigms. You will use all of them.

## The three you need to know

### Event-driven

Code runs in response to things that happen. A user clicks a button. A network request arrives. A timer fires.

```typescript
server.on("request", (request) => {
  sendResponse(request)
})
```

This is JavaScript's natural model. The browser is event-driven. Backend servers are event-driven. You register handlers for events, and the runtime calls them when events occur.

### Functional

Functions are first-class values. You pass them around, compose them, and avoid mutating state.

```typescript
const double = (n: number) => n * 2
const numbers = [1, 2, 3]
const doubled = numbers.map(double) // [2, 4, 6]
```

Key ideas: pure functions (same input, same output, no side effects), immutability (create new data, don't modify existing), and composition (combine small functions into larger ones).

### Object-oriented

Bundle data and behavior into objects.

```typescript
class User {
  constructor(public name: string, public email: string) {}

  greet() {
    return `Hello, ${this.name}`
  }
}

const user = new User("Ada", "ada@example.com")
user.greet() // "Hello, Ada"
```

Key ideas: encapsulation (hide internal details), inheritance (share behavior between types), and polymorphism (use different types through a common interface).

## Why does JS support all three?

Because different problems need different approaches.

- Handling HTTP requests? Event-driven.
- Transforming data? Functional.
- Modeling domain entities (users, orders)? Object-oriented.

You will not pick one. You will use all three in the same project.

## What you will actually do

Most backend TypeScript code looks like this:

```typescript
// Functional: pure transformation
const toUpperCase = (s: string) => s.toUpperCase()

// Event-driven: respond to HTTP request
app.get("/users/:id", async ({ params }) => {
  // Object-oriented: use a class instance
  const user = await userRepository.findById(params.id)
  // Functional: transform the result
  return { name: toUpperCase(user.name) }
})
```

Multi-paradigm means you pick the right tool for each part of the problem.
