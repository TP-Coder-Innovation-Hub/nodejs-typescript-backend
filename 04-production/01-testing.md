[Senior]

# Testing

Bun has a built-in test runner. No extra dependencies needed.

## Testing a function

Create `math.ts`:

```typescript
export const add = (a: number, b: number): number => a + b
export const divide = (a: number, b: number): number => {
  if (b === 0) throw new Error("Division by zero")
  return a / b
}
```

Create `math.test.ts`:

```typescript
import { test, expect, describe } from "bun:test"
import { add, divide } from "./math"

describe("add", () => {
  test("adds two numbers", () => {
    expect(add(2, 3)).toBe(5)
  })

  test("handles negative numbers", () => {
    expect(add(-1, 1)).toBe(0)
  })
})

describe("divide", () => {
  test("divides two numbers", () => {
    expect(divide(10, 2)).toBe(5)
  })

  test("throws on division by zero", () => {
    expect(() => divide(1, 0)).toThrow("Division by zero")
  })
})
```

Run:

```bash
bun test
```

## Matchers

```typescript
expect(value).toBe(expected)            // strict equality (===)
expect(value).toEqual(expected)         // deep equality (objects, arrays)
expect(value).toBeNull()
expect(value).toBeUndefined()
expect(value).toBeDefined()
expect(value).toBeTruthy()
expect(value).toBeFalsy()
expect(array).toContain(item)
expect(array).toHaveLength(3)
expect(fn).toThrow()
expect(fn).toThrow("error message")
expect(promise).resolves.toBe(value)
expect(promise).rejects.toThrow()
```

## Testing an API endpoint

```typescript
import { test, expect, beforeAll, afterAll } from "bun:test"
import { Elysia } from "elysia"

const app = new Elysia()
  .get("/users", () => [{ id: 1, name: "Ada" }])
  .post("/users", ({ body }) => ({ id: 2, ...body }), {
    body: app.t.Object({ name: app.t.String() }),
  })

const BASE = "http://localhost:3999"

let server: any

beforeAll(() => {
  server = app.listen(3999)
})

afterAll(() => {
  server.stop()
})

test("GET /users returns list", async () => {
  const response = await fetch(`${BASE}/users`)
  expect(response.status).toBe(200)
  const users = await response.json()
  expect(users).toHaveLength(1)
  expect(users[0].name).toBe("Ada")
})

test("POST /users creates a user", async () => {
  const response = await fetch(`${BASE}/users`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: "Grace" }),
  })
  expect(response.status).toBe(200)
  const user = await response.json()
  expect(user.name).toBe("Grace")
})
```

## Test file naming

- `*.test.ts` — unit tests
- `*.spec.ts` — specification tests (same thing, different convention)

Bun discovers both patterns automatically.

## What to test

- **Functions** — pure logic, edge cases
- **API endpoints** — status codes, response shapes, error cases
- **Validation** — invalid input is rejected, valid input passes

Do not test framework internals. Test your code.
