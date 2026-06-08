[Mid]

# HTTP and Web Servers

Every backend API communicates over HTTP. Understand the protocol, understand what your server does.

## What is HTTP

HTTP (HyperText Transfer Protocol) is a request-response protocol. A client sends a request, a server sends a response.

```
Client → Request → Server
Client ← Response ← Server
```

## Anatomy of a request

```
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{"name": "Ada", "email": "ada@example.com"}
```

Parts:
- **Method** — what you want to do: `GET`, `POST`, `PUT`, `DELETE`, `PATCH`
- **Path** — which resource: `/users`
- **Headers** — metadata: content type, auth tokens, caching
- **Body** — data (for POST, PUT, PATCH)

## Anatomy of a response

```
HTTP/1.1 201 Created
Content-Type: application/json

{"id": 1, "name": "Ada", "email": "ada@example.com"}
```

Parts:
- **Status code** — what happened: `200` (ok), `201` (created), `400` (bad request), `404` (not found), `500` (server error)
- **Headers** — metadata
- **Body** — the data

## Status codes you will use

| Code | Meaning | When |
|------|---------|------|
| 200 | OK | Successful GET, PUT, DELETE |
| 201 | Created | Successful POST (resource created) |
| 204 | No Content | Successful DELETE (nothing to return) |
| 400 | Bad Request | Invalid input from client |
| 401 | Unauthorized | Missing or invalid auth |
| 403 | Forbidden | Authenticated but not allowed |
| 404 | Not Found | Resource does not exist |
| 500 | Internal Server Error | Something broke on your side |

## What a web server does

A web server listens for HTTP requests and sends responses:

```typescript
import { Elysia } from "elysia"

const app = new Elysia()
  .get("/", () => "Hello")
  .listen(3000)

console.log("Server running on port 3000")
```

Step by step:
1. `new Elysia()` — create a server instance
2. `.get("/", ...)` — register a handler for GET requests to `/`
3. `.listen(3000)` — start listening on port 3000
4. When a request arrives at `/`, the handler runs and the return value is sent as the response

The framework handles parsing the HTTP request, routing it to the correct handler, and serializing the response. You write the handler logic.

## JSON is the default

Modern APIs send and receive JSON. Elysia and Hono parse request bodies as JSON and serialize responses as JSON automatically.

```typescript
app.post("/users", ({ body }) => {
  // body is already parsed as an object
  return { id: 1, ...body }  // automatically serialized to JSON
})
```
