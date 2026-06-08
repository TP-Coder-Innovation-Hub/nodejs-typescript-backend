

# Logging and Monitoring

Logs tell you what happened. Monitoring tells you when something is wrong.

## Structured logging

Print structured JSON, not free-form text. JSON logs are searchable, parseable, and aggregatable.

```typescript
const log = (level: string, message: string, data?: Record<string, unknown>) => {
  console.log(JSON.stringify({
    timestamp: new Date().toISOString(),
    level,
    message,
    ...data,
  }))
}

log("info", "Server started", { port: 3000 })
log("error", "Database connection failed", { error: "ECONNREFUSED", host: "db.example.com" })
```

Output:
```json
{"timestamp":"2024-01-15T10:30:00.000Z","level":"info","message":"Server started","port":3000}
{"timestamp":"2024-01-15T10:30:01.000Z","level":"error","message":"Database connection failed","error":"ECONNREFUSED","host":"db.example.com"}
```

Use libraries like `pino` for production:

```bash
bun add pino
```

```typescript
import pino from "pino"

const logger = pino({
  level: process.env.LOG_LEVEL ?? "info",
})

logger.info({ port: 3000 }, "Server started")
logger.error({ err: error }, "Request failed")
```

## Log levels

| Level | Use for |
|-------|---------|
| error | Something broke. Needs attention now. |
| warn | Unexpected but handled. Might indicate a problem. |
| info | Normal operations. Server started, request completed. |
| debug | Detailed info for debugging. Off in production. |

Set log level via environment variable. In production, use `info`. When debugging, use `debug`.

## Request logging

Log every request:

```typescript
import { Elysia } from "elysia"

const app = new Elysia()
  .onRequest(({ request }) => {
    logger.info({ method: request.method, url: request.url }, "Request received")
  })
  .onAfterResponse(({ request, set }) => {
    logger.info({ method: request.method, url: request.url, status: set.status }, "Response sent")
  })
  .get("/users", () => getUsers())
  .listen(3000)
```

## Health checks

Expose an endpoint that returns the server status:

```typescript
app.get("/health", () => {
  return {
    status: "ok",
    uptime: process.uptime(),
    timestamp: new Date().toISOString(),
  }
})
```

Deploy tools (Kubernetes, load balancers) hit this endpoint. If it returns 200, the server is alive. If it doesn't, the server gets restarted.

For deeper checks:

```typescript
app.get("/health", async () => {
  const dbOk = await checkDatabaseConnection()
  return {
    status: dbOk ? "ok" : "degraded",
    uptime: process.uptime(),
    database: dbOk ? "connected" : "disconnected",
  }
})
```

## Monitoring

Logging is passive. Monitoring is active alerting.

- **Metrics** — request count, response time, error rate. Use a service like Prometheus + Grafana, or a hosted solution.
- **Alerts** — notify when error rate spikes, response time increases, or health checks fail.
- **Tracing** — follow a request across services. Use OpenTelemetry.

Start with logging and health checks. Add metrics and alerts when you have users.
