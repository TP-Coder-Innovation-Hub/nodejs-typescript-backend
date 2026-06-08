

# Validation

Never trust input. Validate everything that comes from outside your code — request bodies, query parameters, path parameters, headers.

## Why validate

- **Client sends wrong types.** A string where you expect a number. A missing required field.
- **Client sends malicious data.** SQL injection, XSS, oversized payloads.
- **Your code assumes a shape.** If the data doesn't match, you get runtime errors in unexpected places.

Validate at the boundary. Once data passes validation, your internal code can trust it.

## Elysia's `t` (TypeBox)

Elysia includes TypeBox for runtime validation with type inference:

```typescript
import { Elysia, t } from "elysia"

const app = new Elysia()
  .post(
    "/users",
    ({ body }) => {
      // body is typed as { name: string; email: string; age: number }
      return { id: 1, ...body }
    },
    {
      body: t.Object({
        name: t.String({ minLength: 1, maxLength: 100 }),
        email: t.String({ format: "email" }),
        age: t.Number({ minimum: 0, maximum: 150 }),
      }),
    }
  )
```

Step by step:
1. `t.Object({...})` defines the schema.
2. Elysia validates the request body against this schema before the handler runs.
3. If validation fails, the client gets a 400 error with details about what failed.
4. Inside the handler, `body` is fully typed. TypeScript knows every field and its type.

## Common validators

```typescript
t.String()                           // any string
t.String({ minLength: 1 })          // non-empty string
t.String({ format: "email" })       // email format
t.String({ format: "uuid" })        // UUID format
t.Number()                           // any number
t.Number({ minimum: 0 })            // non-negative
t.Boolean()                          // true or false
t.Array(t.String())                  // string[]
t.Optional(t.String())              // field can be omitted
t.Union([t.Literal("active"), t.Literal("inactive")])  // "active" | "inactive"
```

## Reusable schemas

Define schemas once, reuse across routes:

```typescript
const CreateUserSchema = t.Object({
  name: t.String({ minLength: 1 }),
  email: t.String({ format: "email" }),
})

const app = new Elysia()
  .post("/users", ({ body }) => createUser(body), { body: CreateUserSchema })
  .put("/users/:id", ({ body }) => updateUser(body), { body: CreateUserSchema })
```

## zod (alternative)

If you prefer zod:

```bash
bun add zod
```

```typescript
import { z } from "zod"

const CreateUserSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
  age: z.number().int().min(0).optional(),
})

type CreateUser = z.infer<typeof CreateUserSchema>
```

zod is popular and works everywhere (not tied to a framework). Elysia's `t` is built-in and has zero extra dependencies. Either works. Pick one.

## The rule

Validate at the edge. Define a schema. Let the schema generate types. Never write the same shape twice (once as a type, once as a validator). Infer from the schema.
