# AGENTS.md

This file provides context for AI agents (and developers) working in this repository.

## Context

This is the **Node.js & TypeScript Backend** learning path repository under the TP-Coder Innovation Hub. It teaches backend engineering using Node.js and TypeScript, covering fundamentals through production deployment. The content targets self-directed learners working through hands-on modules.

## Audience

Three tiers of experience:

- **Entry**: New to backend development or new to Node.js. Needs clear explanations, annotated code examples, and links to foundational concepts. Avoid assuming knowledge of event-driven architecture or type systems.
- **Mid**: Has built basic APIs. Needs guidance on testing, databases, authentication, observability, and production patterns. Can read framework documentation independently.
- **Senior**: Comfortable building and deploying services. Needs depth on performance optimization, distributed systems, advanced TypeScript patterns, and architectural decision-making.

Each module and code example is tagged with its target level: `[Entry]`, `[Mid]`, `[Senior]`.

## How to Help

- Write **compilable, runnable code**. Every code block should work if copied into a properly configured project.
- Use **ESM module syntax** (`import`/`export`) exclusively. No CommonJS (`require`, `module.exports`).
- Use **strict TypeScript** (`strict: true`). Provide types for all function parameters and return values.
- Prefer **Fastify** for HTTP server examples. Use Hono for edge/serverless examples. Use Express only when discussing legacy patterns.
- Use **Drizzle ORM** for database examples unless the module specifically covers Prisma.
- Write tests with **Vitest**. Include both unit and integration test patterns.
- Use **Biome** for linting and formatting in all project setup examples.
- Include **error handling** in every non-trivial code example. Show the happy path and the error path.
- Annotate code with inline comments for `[Entry]` level content. Use minimal comments for `[Senior]` level content.
- When explaining concepts, reference the relevant section of `README.md` or link to official documentation.
- Use `zod` for runtime input validation alongside TypeScript static types.

## How NOT to Help

- Do not introduce CommonJS patterns in new code. If showing a CommonJS example for migration purposes, clearly label it as legacy.
- Do not use `any` type. If a type is truly unknown, use `unknown` and narrow it.
- Do not skip error handling in examples because "it's just a demo." Learners copy what they see.
- Do not recommend deprecated or unmaintained packages. Check npm for last publish date and maintenance status.
- Do not use framework-specific decorators or magic without explaining what they do. Entry-level learners will not understand `@Injectable()` or similar abstractions without context.
- Do not assume the learner has a specific operating system, editor, or deployment target. Provide cross-platform instructions.
- Do not use environment-specific APIs without noting the required Node.js version or runtime.
- Do not write overly clever code. Prefer readability over elegance. This is educational content.

## Key Concepts

These are the foundational concepts this learning path teaches. All content should reinforce understanding of these ideas:

1. **Event-Driven Architecture**: The Node.js event loop, non-blocking I/O, callback/promise/async-await patterns, and the implications of single-threaded execution.
2. **Type Safety at Scale**: TypeScript's type system as a development tool (not just error prevention). Compile-time guarantees, type narrowing, generics, branded types, and schema validation with `zod`.
3. **ESM Module System**: Modern JavaScript module syntax, `import.meta.url`, tree-shaking, and the migration from CommonJS. No new code should use `require()`.
4. **Structured Error Handling**: Typed errors, error middleware, error propagation patterns, and the difference between operational errors (expected) and programmer errors (bugs).
5. **Testing Discipline**: Unit tests, integration tests, test organization, mocking strategies, and how to write tests that give confidence without being brittle.
6. **Production Readiness**: Logging (structured), monitoring (metrics), health checks, graceful shutdown, and deployment considerations (containerization, serverless).
7. **Security Fundamentals**: Input validation, SQL injection prevention (parameterized queries via ORM), authentication patterns, and dependency auditing.

## Node.js / TypeScript Guidelines (2026)

These are the technology choices and conventions for this learning path:

### Runtime and Language

- **Node.js 22+ LTS** (or Bun as alternative runtime)
- **TypeScript 5.5+** with `strict: true`
- **ESM modules** (`"type": "module"` in `package.json`)
- **`tsconfig.json`**: `target: "ES2024"`, `module: "NodeNext"`, `moduleResolution: "NodeNext"`

### HTTP Frameworks

- **Default recommendation**: Fastify (for standard Node.js deployment)
- **Edge/serverless**: Hono
- **Enterprise/large-team**: NestJS (note: NestJS has its own conventions; follow NestJS patterns when using it)

### Database Access

- **Default ORM**: Drizzle ORM (lightweight, type-safe, SQL-first)
- **Alternative**: Prisma (when the project benefits from its schema-first approach and migration tooling)
- Always use parameterized queries. Never concatenate user input into SQL strings.

### Testing

- **Test runner**: Vitest
- **Assertion style**: Vitest built-in assertions (`expect`)
- **Mocking**: `vi.fn()`, `vi.mock()`, `vi.spyOn()`
- **Coverage**: `vitest --coverage` with V8 or Istanbul provider
- **Test file location**: Co-located (`*.test.ts`) or in `__tests__/` directories -- be consistent within a module

### Linting and Formatting

- **Tool**: Biome (replaces ESLint + Prettier)
- **Configuration**: `biome.json` in project root
- **Run in CI**: `biome ci .`

### Validation

- **Runtime validation**: `zod`
- **Pattern**: Define a Zod schema, infer the TypeScript type with `z.infer<>`, validate at the boundary (request parsing), trust the type inside.

### Error Handling

- Use structured error classes that extend a base `AppError` class.
- Include error `code`, `statusCode`, and human-readable `message`.
- Use framework-level error middleware (Fastify error handler, Hono error handler).
- Never expose stack traces or internal error details in production responses.
- Log errors with structured logging (JSON format) including request context.

### Project Structure (Per Module)

```
module-NN-name/
  src/
    index.ts          # Entry point
    routes/           # Route handlers
    services/         # Business logic
    db/               # Database schema and client
    middleware/       # Custom middleware
    errors/           # Error classes and handlers
    types/            # Shared TypeScript types
  tests/
    *.test.ts         # Test files
  package.json
  tsconfig.json
  biome.json
  drizzle.config.ts   # If module uses a database
```

### Package Manager

- **pnpm** is the recommended package manager for this learning path.
- All `package.json` examples should include exact version numbers (no `^` or `~`) for reproducibility.
- Include `pnpm-lock.yaml` in version control.

## Repository Structure

```
nodejs-typescript-backend/
  README.md              # This fundamentals guide
  AGENTS.md              # This file
  assets/                # Diagrams and images
  module-01-setup/       # Project setup module
  module-02-rest-api/    # REST API module
  module-03-database/    # Database access module
  module-04-auth/        # Authentication module
  module-05-testing/     # Testing module
  module-06-errors/      # Error handling module
  module-07-realtime/    # Real-time communication module
  module-08-performance/ # Performance module
  module-09-deploy/      # Deployment module
  module-10-microservices/ # Microservices module
```

Module numbering uses zero-padded two-digit format (`module-01-`, `module-02-`, etc.). Module directory names use kebab-case.
