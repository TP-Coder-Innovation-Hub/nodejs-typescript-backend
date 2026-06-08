

# Setup

Install Bun, pick an editor, run your first program.

## Install Bun

**macOS / Linux:**
```bash
curl -fsSL https://bun.sh/install | bash
```

**Windows:**
```bash
powershell -c "irm bun.sh/install.ps1 | iex"
```

**Verify:**
```bash
bun --version
```

## Editor

Use VS Code. Install the TypeScript extension (built-in) and the Bun extension for intellisense.

Other options: WebStorm, Zed, Neovim. All work. VS Code has the best TypeScript support.

## Your first program

Create a file called `index.ts`:

```typescript
console.log("Hello from Bun!")
```

Run it:

```bash
bun run index.ts
```

Output: `Hello from Bun!`

No build step. No `package.json`. No config. Write TypeScript, run it.

## Create a project

For a real project, initialize properly:

```bash
mkdir my-app && cd my-app
bun init
```

This creates a `package.json` and `tsconfig.json`. The `package.json` tracks dependencies. The `tsconfig.json` configures TypeScript.

## Install a dependency

```bash
bun add elysia
```

Bun installs packages into `node_modules` and updates `package.json`. Faster than npm, yarn, or pnpm.

## Run scripts

In `package.json`:

```json
{
  "scripts": {
    "dev": "bun run --watch index.ts"
  }
}
```

```bash
bun run dev
```

The `--watch` flag restarts the process when files change. No manual restart needed.

## What you have now

- Bun runtime (runs TypeScript directly)
- A code editor with TypeScript support
- A project with package management
- Hot reload for development

That is the entire setup. No webpack, no babel, no compile step. Bun handles it all.
