[Senior]

# Capstone Project

Build a type-safe task management API. This project uses everything you learned.

## Requirements

Build a REST API for managing tasks with these endpoints:

```
GET    /tasks           — list all tasks (with optional ?status= filter)
GET    /tasks/:id       — get a single task
POST   /tasks           — create a task
PATCH  /tasks/:id       — update a task
DELETE /tasks/:id       — delete a task
```

## Stack

- **Runtime:** Bun
- **Framework:** Elysia
- **Database:** SQLite with Drizzle ORM
- **Validation:** Elysia's `t` (TypeBox)
- **Testing:** Bun test runner

## Data model

```typescript
type Task = {
  id: number
  title: string
  description: string | null
  status: "todo" | "in_progress" | "done"
  createdAt: string
  updatedAt: string
}
```

## Steps

1. **Initialize the project**

```bash
mkdir task-api && cd task-api
bun init
bun add elysia drizzle-orm better-sqlite3
bun add -d drizzle-kit
```

2. **Define the database schema**

Create `src/db/schema.ts` with a `tasks` table. Columns: `id` (auto-increment primary key), `title` (text, not null), `description` (text, nullable), `status` (text, not null, default "todo"), `created_at` (text), `updated_at` (text).

Derive `Task` and `NewTask` types from the schema using `$inferSelect` and `$inferInsert`.

3. **Create the database connection**

Create `src/db/index.ts`. Initialize Drizzle with the SQLite connection and the schema.

4. **Build the API**

Create `src/index.ts` with Elysia:

- `GET /tasks` — `db.select().from(tasks)`. Support `?status=todo` query parameter for filtering. Use `eq` from `drizzle-orm` for the where clause.
- `GET /tasks/:id` — `db.select().from(tasks).where(eq(tasks.id, id))`. Return 404 if not found.
- `POST /tasks` — `db.insert(tasks).values(body).returning()`. Validate body with `t.Object({ title: t.String({ minLength: 1 }), description: t.Optional(t.String()) })`.
- `PATCH /tasks/:id` — `db.update(tasks).set(body).where(eq(tasks.id, id))`. Validate body with partial schema (all fields optional).
- `DELETE /tasks/:id` — `db.delete(tasks).where(eq(tasks.id, id))`. Return 204 on success.
- Error handler for 404s and validation errors.

5. **Write tests**

Create `src/index.test.ts`:

- Test `GET /tasks` returns an array
- Test `POST /tasks` creates a task and returns it with an id
- Test `POST /tasks` rejects empty title
- Test `GET /tasks/:id` returns a single task
- Test `GET /tasks/:id` returns 404 for missing task
- Test `DELETE /tasks/:id` removes the task

Use a separate test database. Reset it between tests.

6. **Add configuration**

Create `src/config.ts`. Load `PORT`, `DATABASE_URL`, `LOG_LEVEL` from environment variables with defaults.

7. **Add logging**

Log each request (method, path, status, duration). Use structured JSON logging.

8. **Add a health check**

`GET /health` returns `{ status: "ok", uptime, timestamp }`.

## Success criteria

- All endpoints return correct status codes
- TypeScript compiles with no errors
- All tests pass (`bun test`)
- Invalid input returns 400 with error details
- Each request is logged
- Health check works
