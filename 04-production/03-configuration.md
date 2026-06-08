[Senior]

# Configuration

Configuration belongs outside your code. Environment variables and `.env` files are the standard.

## Environment variables

Bun reads environment variables through `process.env` and `Bun.env`:

```typescript
const port = Number(process.env.PORT) || 3000
const dbUrl = process.env.DATABASE_URL!
```

`Bun.env` is the same thing:

```typescript
const port = Number(Bun.env.PORT) || 3000
```

## .env files

Create `.env`:

```
PORT=3000
DATABASE_URL=file:./dev.db
LOG_LEVEL=debug
JWT_SECRET=local-dev-secret
```

Bun loads `.env` automatically. No library needed. No `dotenv` package. Bun reads `.env` when the process starts.

Never commit `.env`. Add it to `.gitignore`:

```
.env
.env.local
```

## .env for different environments

```
.env              — default (committed, no secrets)
.env.local        — local overrides (not committed, has secrets)
.env.production   — production values (not committed)
```

## Typed configuration

Parse and validate config at startup. Fail fast if something is missing:

```typescript
type Config = {
  port: number
  databaseUrl: string
  logLevel: "debug" | "info" | "warn" | "error"
  jwtSecret: string
}

function loadConfig(): Config {
  const port = Number(Bun.env.PORT) || 3000
  const databaseUrl = Bun.env.DATABASE_URL
  const logLevel = Bun.env.LOG_LEVEL ?? "info"
  const jwtSecret = Bun.env.JWT_SECRET

  if (!databaseUrl) {
    throw new Error("DATABASE_URL is required")
  }
  if (!jwtSecret) {
    throw new Error("JWT_SECRET is required")
  }

  const validLevels = ["debug", "info", "warn", "error"]
  if (!validLevels.includes(logLevel)) {
    throw new Error(`LOG_LEVEL must be one of: ${validLevels.join(", ")}`)
  }

  return { port, databaseUrl, logLevel, jwtSecret }
}

export const config = loadConfig()
```

Use everywhere:

```typescript
import { config } from "./config"

app.listen(config.port)
logger.level(config.logLevel)
db.connect(config.databaseUrl)
```

## Secrets

Secrets (passwords, API keys, tokens) go in:
- **Local dev:** `.env.local` (not committed)
- **CI/CD:** CI secrets (GitHub Actions secrets, etc.)
- **Production:** hosted secrets manager (AWS Secrets Manager, etc.)

Never hardcode secrets. Never commit them. Never log them.
