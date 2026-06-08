[Mid]

# Your First API

Build a type-safe REST API with Elysia. Every line explained.

## Setup

```bash
mkdir my-api && cd my-api
bun init
bun add elysia
```

## The code

Create `index.ts`:

```typescript
import { Elysia, t } from "elysia"

type User = {
  id: number
  name: string
  email: string
}

const users: User[] = [
  { id: 1, name: "Ada", email: "ada@example.com" },
  { id: 2, name: "Grace", email: "grace@example.com" },
]

let nextId = 3

const app = new Elysia()
  .get("/users", () => users)

  .get("/users/:id", ({ params: { id } }) => {
    const user = users.find((u) => u.id === id)
    if (!user) {
      throw new Error(`User ${id} not found`)
    }
    return user
  })

  .post(
    "/users",
    ({ body }) => {
      const user: User = { id: nextId++, ...body }
      users.push(user)
      return user
    },
    {
      body: t.Object({
        name: t.String({ minLength: 1 }),
        email: t.String({ format: "email" }),
      }),
    }
  )

  .delete("/users/:id", ({ params: { id } }) => {
    const index = users.findIndex((u) => u.id === id)
    if (index === -1) {
      throw new Error(`User ${index} not found`)
    }
    users.splice(index, 1)
    return { message: "Deleted" }
  })

  .onError(({ error }) => {
    return { error: error.message }
  })

  .listen(3000)

console.log(`API running at http://localhost:${app.server!.port}`)
```

## Line by line

`import { Elysia, t } from "elysia"` — Elysia is the framework. `t` is the type validator (powered by TypeBox).

`type User = { ... }` — define the shape of a user object. TypeScript enforces this at compile time.

`const users: User[] = [...]` — in-memory data store. In a real app, this would be a database.

`.get("/users", () => users)` — register a GET handler at `/users`. Returns the full list. Elysia serializes the array to JSON.

`.get("/users/:id", ({ params: { id } }) => { ... })` — `:id` is a path parameter. Elysia parses it and passes it in `params`. TypeScript knows `id` is a `number` because Elysia infers it.

`.post("/users", ({ body }) => { ... }, { body: t.Object({...}) })` — the third argument is a schema. Elysia validates the request body against this schema before the handler runs. If validation fails, the client gets a 400 error with details. The handler only runs with valid data.

`.delete("/users/:id", ...)` — remove a user by ID. `splice` modifies the array in place.

`.onError(({ error }) => { ... })` — catch unhandled errors and return a consistent error shape. Without this, unhandled errors become 500 responses with no details.

`.listen(3000)` — start the server on port 3000.

## Run

```bash
bun run index.ts
```

## Test with curl

```bash
curl http://localhost:3000/users
curl http://localhost:3000/users/1
curl -X POST http://localhost:3000/users -H "Content-Type: application/json" -d '{"name":"Linus","email":"linus@example.com"}'
curl -X DELETE http://localhost:3000/users/1
```

You have a working REST API with type-safe routing, validation, and error handling. Every endpoint is checked by TypeScript at compile time and by Elysia's validator at runtime.
