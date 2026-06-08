[Mid]

# REST API Design

REST (Representational State Transfer) is a convention for organizing HTTP APIs. It is not a strict standard. It is a set of patterns that make APIs predictable.

## Resources and URLs

A resource is a thing: users, products, orders. URLs identify resources.

```
GET    /users          — list users
GET    /users/1        — get user 1
POST   /users          — create a user
PUT    /users/1        — replace user 1
PATCH  /users/1        — update parts of user 1
DELETE /users/1        — delete user 1
```

The URL is a noun (the resource). The HTTP method is the verb (the action).

## The CRUD mapping

| Operation | Method | Path | Status | Response |
|-----------|--------|------|--------|----------|
| Create | POST | /users | 201 | Created resource |
| Read (list) | GET | /users | 200 | Array of resources |
| Read (one) | GET | /users/1 | 200 | Single resource |
| Update | PUT or PATCH | /users/1 | 200 | Updated resource |
| Delete | DELETE | /users/1 | 204 | Empty |

## Nested resources

Relationships between resources:

```
GET    /users/1/orders          — list orders for user 1
GET    /users/1/orders/42       — get order 42 for user 1
POST   /users/1/orders          — create an order for user 1
```

Keep nesting shallow. One level deep is fine. Two levels is pushing it. Beyond that, restructure.

## Query parameters for filtering and pagination

```
GET /users?status=active&page=2&limit=20
```

- `status=active` — filter
- `page=2&limit=20` — pagination
- `sort=created_at&order=desc` — sorting

Query parameters are for optional modifiers. Path parameters are for identifying resources.

## Response shape

Consistency matters. Always return the same structure:

```typescript
// Single resource
{
  "id": 1,
  "name": "Ada",
  "email": "ada@example.com"
}

// List with pagination
{
  "data": [
    { "id": 1, "name": "Ada", "email": "ada@example.com" }
  ],
  "total": 42,
  "page": 1,
  "limit": 20
}

// Error
{
  "error": "Not Found",
  "message": "User 999 does not exist"
}
```

## Naming rules

- Use plural nouns: `/users`, not `/user`
- Use lowercase: `/users`, not `/Users`
- Use hyphens for multi-word: `/user-profiles`, not `/userProfiles`
- No verbs in URLs: `/users`, not `/getUsers`

## When REST is not enough

REST works for CRUD. It breaks down for:
- **Actions** — "send email," "reset password." Use `POST /users/1/send-email` or a separate endpoint.
- **Complex queries** — multiple filters, joins. Consider GraphQL or dedicated search endpoints.
- **Real-time** — WebSockets, Server-Sent Events. REST is request-response only.
