

# Database Access

Use Drizzle ORM for type-safe database queries. SQL knowledge transfers directly.

## Setup

```bash
bun add drizzle-orm
bun add -d drizzle-kit
```

Drizzle works with many databases. This example uses SQLite (Bun has built-in support):

```bash
bun add better-sqlite3
```

## Define a schema

Create `src/db/schema.ts`:

```typescript
import { sqliteTable, text, integer } from "drizzle-orm/sqlite-core"

export const users = sqliteTable("users", {
  id: integer("id").primaryKey({ autoIncrement: true }),
  name: text("name").notNull(),
  email: text("email").notNull().unique(),
  createdAt: text("created_at").notNull().$defaultFn(() => new Date().toISOString()),
})

export type User = typeof users.$inferSelect
export type NewUser = typeof users.$inferInsert
```

`sqliteTable` defines a table. Each column has a type and constraints. `$inferSelect` and `$inferInsert` derive TypeScript types from the schema. You write the schema once, TypeScript knows the types everywhere.

## Connect

Create `src/db/index.ts`:

```typescript
import { drizzle } from "drizzle-orm/better-sqlite3"
import Database from "better-sqlite3"
import * as schema from "./schema"

const sqlite = new Database("app.db")
export const db = drizzle(sqlite, { schema })
```

## Query

```typescript
import { eq } from "drizzle-orm"
import { db } from "./db"
import { users } from "./db/schema"

// SELECT * FROM users
const allUsers = await db.select().from(users)

// SELECT * FROM users WHERE id = 1
const user = await db.select().from(users).where(eq(users.id, 1))

// INSERT INTO users (name, email) VALUES ('Ada', 'ada@example.com')
const [newUser] = await db.insert(users).values({
  name: "Ada",
  email: "ada@example.com",
}).returning()

// UPDATE users SET name = 'Ada Lovelace' WHERE id = 1
await db.update(users).set({ name: "Ada Lovelace" }).where(eq(users.id, 1))

// DELETE FROM users WHERE id = 1
await db.delete(users).where(eq(users.id, 1))
```

Every query is type-safe. If you try to insert a number where a string is expected, TypeScript catches it. If you miss a required field, TypeScript catches it.

## Relations

Define relationships between tables:

```typescript
import { relations } from "drizzle-orm"
import { users, posts } from "./schema"

export const usersRelations = relations(users, ({ many }) => ({
  posts: many(posts),
}))

export const posts = sqliteTable("posts", {
  id: integer("id").primaryKey({ autoIncrement: true }),
  title: text("title").notNull(),
  userId: integer("user_id").notNull().references(() => users.id),
})

export const postsRelations = relations(posts, ({ one }) => ({
  author: one(users, {
    fields: [posts.userId],
    references: [users.id],
  }),
}))
```

Query with joins:

```typescript
const usersWithPosts = await db.query.users.findMany({
  with: { posts: true },
})
```

## Why Drizzle

- Queries look like SQL. You learn SQL while writing TypeScript.
- Type inference from schema. No duplicate type definitions.
- No magic. No query builder abstraction. You see the SQL you write.
