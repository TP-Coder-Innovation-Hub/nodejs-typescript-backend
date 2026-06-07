# AGENTS.md

This file provides context for AI agents (and developers) working in this repository.

## Context

This is the **JavaScript & TypeScript Backend** learning path repository under the TP-Coder Innovation Hub. It teaches backend engineering using JavaScript/TypeScript with Bun as the primary runtime and Elysia/Hono as the framework stack. The content targets self-directed learners working through hands-on modules.

## Audience

Three tiers of experience:

- **Entry**: New to backend development or new to JS/TS. Needs clear explanations, annotated code examples, and links to foundational concepts. Avoid assuming knowledge of event-driven architecture or type systems.
- **Mid**: Has built basic APIs. Needs guidance on testing, databases, authentication, observability, and production patterns. Can read framework documentation independently.
- **Senior**: Comfortable building and deploying services. Needs depth on performance optimization, distributed systems, advanced TypeScript patterns, and architectural decision-making.

Each module and code example is tagged with its target level: `[Entry]`, `[Mid]`, `[Senior]`.

## How to Help

- Write **compilable, runnable code**. Every code block should work if copied into a properly configured project.
- Use **ESM module syntax** (`import`/`export`) exclusively. No CommonJS (`require`, `module.exports`).
- Use **strict TypeScript** (`strict: true`). Provide types for all function parameters and return values.
- **Runtime**: Default to **Bun** for all examples. Note Node.js differences only when relevant.
- **Framework**: Use **Elysia** for Bun-native examples. Use **Hono** for multi-runtime/edge examples.
- Use **Drizzle ORM** for database examples.
- Write tests with **Bun's built-in test runner** (`bun test`). Use `expect()` from Bun's built-in assertions.
- Use **Biome** for linting and formatting in all project setup examples.
- Include **error handling** in every non-trivial code example. Show the happy path and the error path.
- Annotate code with inline comments for `[Entry]` level content. Use minimal comments for `[Senior]` level content.
- When explaining concepts, reference the relevant section of `README.md` or link to official documentation.
- Use **Elysia's built-in type system** (`t.Object`, `t.String`, etc.) for schema validation in Elysia examples. Use **zod** for Hono examples.

## How NOT to Help

- Do not introduce CommonJS patterns in new code. If showing a CommonJS example for migration purposes, clearly label it as legacy.
- Do not use `any` type. If a type is truly unknown, use `unknown` and narrow it.
- Do not skip error handling in examples because "it's just a demo." Learners copy what they see.
- Do not recommend Express for new projects. It is legacy. Mention it only for context.
- Do not recommend deprecated or unmaintained packages. Check npm for last publish date and maintenance status.
- Do not assume Node.js as the runtime unless the learner specifically mentions it. Default to Bun.
- Do not use `npm` or `pnpm` commands unless the learner is on Node.js. Use `bun add`, `bun install`, etc.
- Do not write overly clever code. Prefer readability over elegance. This is educational content.

## Key Concepts

These are the foundational concepts this learning path teaches. All content should reinforce understanding of these ideas:

1. **Event-Driven Architecture**: The JS event loop, non-blocking I/O, callback/promise/async-await patterns, and the implications of single-threaded execution. This is the same across Bun, Node.js, and Deno.
2. **Type Safety at Scale**: TypeScript's type system as a development tool. End-to-end type safety with Elysia schemas: one schema definition drives runtime validation, compile-time types, and API documentation.
3. **The Bun Runtime**: Native TypeScript, built-in toolchain (package manager, test runner, bundler), web standard APIs, and performance characteristics. When Bun vs Node.js.
4. **Structured Error Handling**: Typed errors, error middleware, error propagation, and the difference between operational errors (expected) and programmer errors (bugs).
5. **Testing Discipline**: Bun's built-in test runner, unit/integration tests, mocking strategies, and writing tests that give confidence without being brittle.
6. **Production Readiness**: Structured logging, monitoring, health checks, graceful shutdown, Docker containerization, and deployment strategies.
7. **Security Fundamentals**: Input validation, SQL injection prevention, authentication patterns (JWT, sessions), and dependency auditing.

## JavaScript/TypeScript Backend Guidelines (2026)

These are the technology choices and conventions for this learning path:

### Runtime and Language

- **Bun 1.x** as the primary runtime
- **TypeScript 5.5+** with `strict: true`
- **ESM modules** (default in Bun, no config needed)

### HTTP Frameworks

- **Default for Bun**: Elysia (end-to-end type safety, Bun-native)
- **Default for multi-runtime/edge**: Hono (runs everywhere)
- **Legacy Node.js**: Fastify (only when maintaining existing Node.js projects)

### Database Access

- **Default ORM**: Drizzle ORM (lightweight, type-safe, SQL-first)
- Always use parameterized queries. Never concatenate user input into SQL strings.

### Testing

- **Test runner**: Bun's built-in test runner (`bun test`)
- **Assertion style**: `expect()` from `bun:test`
- **Test file location**: Co-located (`*.test.ts`) — be consistent within a module

### Linting and Formatting

- **Tool**: Biome (replaces ESLint + Prettier)
- **Configuration**: `biome.json` in project root

### Validation

- **Elysia**: Built-in `t` schema (Elysia's type system)
- **Hono**: `zod` with `@hono/zod-validator`
- **Pattern**: Define schema, infer TypeScript type, validate at boundary, trust the type inside.

### Error Handling

- Use Elysia's `onError` hook or Hono's error middleware.
- Include error `code`, `statusCode`, and human-readable `message`.
- Never expose stack traces or internal error details in production responses.
- Log errors with structured logging (JSON format) including request context.

### Package Manager

- **bun** is the package manager. Use `bun add`, `bun install`, `bun run`.
- Do not use npm, yarn, or pnpm commands unless specifically working on a Node.js project.

### Project Structure (Per Module)

```
module-NN-name/
  src/
    index.ts          # Entry point
    routes/           # Route handlers
    services/         # Business logic
    db/               # Database schema and client
    middleware/       # Custom middleware / plugins
    errors/           # Error classes and handlers
    types/            # Shared TypeScript types
  tests/
    *.test.ts         # Test files
  package.json
  tsconfig.json
  biome.json
```

## Repository Structure

```
javascript-typescript-backend/
  README.md              # This fundamentals guide
  AGENTS.md              # This file
  assets/                # Diagrams and images
  module-01-setup/       # Project setup module
  module-02-elysia/      # Building REST API with Elysia
  module-03-hono/        # Building REST API with Hono
  module-04-database/    # Database access with Drizzle
  module-05-auth/        # Authentication module
  module-06-testing/     # Testing with Bun
  module-07-errors/      # Error handling and observability
  module-08-realtime/    # Real-time communication
  module-09-performance/ # Performance optimization
  module-10-deploy/      # Deployment and production
```

Module numbering uses zero-padded two-digit format (`module-01-`, `module-02-`, etc.). Module directory names use kebab-case.
