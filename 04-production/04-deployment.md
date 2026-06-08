

# Deployment

Get your API from your machine to a server.

## Build with Bun

Bun can bundle your entire app into a single executable:

```bash
bun build ./src/index.ts --compile --outfile my-api
```

This produces a single binary. No runtime needed on the target machine. No `node_modules`. Just the binary.

Run it:

```bash
./my-api
```

For a JavaScript bundle (not compiled binary):

```bash
bun build ./src/index.ts --outdir ./dist --target=bun
```

## Docker

Create `Dockerfile`:

```dockerfile
FROM oven/bun:1 AS base
WORKDIR /app

FROM base AS install
COPY package.json bun.lockb ./
RUN bun install --frozen-lockfile

FROM base AS release
COPY --from=install /app/node_modules ./node_modules
COPY . .

ENV PORT=3000
EXPOSE 3000

CMD ["bun", "run", "src/index.ts"]
```

Build and run:

```bash
docker build -t my-api .
docker run -p 3000:3000 -e DATABASE_URL=... my-api
```

## Multi-stage for compiled binary

Smaller image, no Bun runtime in the final stage:

```dockerfile
FROM oven/bun:1 AS build
WORKDIR /app
COPY . .
RUN bun build ./src/index.ts --compile --outfile my-api

FROM debian:bookworm-slim
COPY --from=build /app/my-api /app/my-api
EXPOSE 3000
CMD ["/app/my-api"]
```

## Deployment options

**Platform-as-a-Service (simplest):** Railway, Fly.io, Render. Push code, they build and run. Good for small to medium apps.

**Container hosting:** AWS ECS, Google Cloud Run, Azure Container Apps. Push a Docker image, they run it. Good for production workloads.

**Virtual machine:** Rent a VPS (DigitalOcean, Hetzner), SSH in, run the binary or Docker container. Full control, more maintenance.

**Serverless:** For Bun, support is emerging. AWS Lambda supports container images. Cloudflare Workers supports a subset of Bun APIs via Hono.

## Production checklist

- Set `LOG_LEVEL=info` (not debug)
- Set a strong `JWT_SECRET` (32+ random characters)
- Set `DATABASE_URL` to the production database
- Enable HTTPS (via reverse proxy like Nginx, or the hosting platform)
- Set health check endpoint (`/health`)
- Set restart policy (restart on crash)
- Set resource limits (memory, CPU)
- Back up the database

## Process management

Use a process manager to keep the app running:

```bash
# systemd (Linux)
[Unit]
Description=My API
After=network.target

[Service]
ExecStart=/app/my-api
Restart=always
Environment=PORT=3000
Environment=DATABASE_URL=...
Environment=JWT_SECRET=...

[Install]
WantedBy=multi-user.target
```

Or use Docker with `restart: always` in `docker-compose.yml`:

```yaml
services:
  api:
    build: .
    ports:
      - "3000:3000"
    env_file: .env.production
    restart: always
```
