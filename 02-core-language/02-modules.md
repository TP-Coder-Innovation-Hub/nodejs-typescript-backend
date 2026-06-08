[Mid]

# Modules

Modules let you split code into files. Each file is a module. Export what other files need, import what you need from others.

## ESM — the standard

ECMAScript Modules (ESM) use `import` and `export`:

```typescript
// math.ts — export
export const add = (a: number, b: number): number => a + b
export const multiply = (a: number, b: number): number => a * b
```

```typescript
// main.ts — import
import { add, multiply } from "./math"

add(2, 3)       // 5
multiply(2, 3)  // 6
```

## Named exports vs default exports

**Named exports** — export multiple things by name:
```typescript
// utils.ts
export const trim = (s: string) => s.trim()
export const capitalize = (s: string) => s[0].toUpperCase() + s.slice(1)
```

```typescript
import { trim, capitalize } from "./utils"
```

**Default export** — export one main thing:
```typescript
// logger.ts
export default function log(message: string) {
  console.log(`[${new Date().toISOString()}] ${message}`)
}
```

```typescript
import log from "./logger"
```

Prefer named exports. They make it explicit what you are importing. Default exports force you to pick a name on the import side, which leads to inconsistent naming across files.

## Re-exports

Re-export from another module:

```typescript
// api/index.ts
export { getUsers, createUser } from "./users"
export { getProducts, createProduct } from "./products"
```

Consumers import from one place:
```typescript
import { getUsers, getProducts } from "./api"
```

## Why ESM

ESM is the JavaScript standard. It enables tree-shaking — bundlers remove unused exports, reducing file size. It is statically analyzable: tools know your dependencies without running code.

## CommonJS — the legacy format

Before ESM, Node.js used CommonJS:

```javascript
const { add } = require("./math")
module.exports = { add, multiply }
```

CommonJS is synchronous and cannot be tree-shaken. It is still present in older packages. Bun supports both ESM and CommonJS, but you should write ESM.

## Enabling ESM

In `package.json`:

```json
{
  "type": "module"
}
```

This tells the runtime to treat `.js` files as ESM. With Bun and `.ts` files, ESM is the default — no configuration needed.

## Module resolution

```typescript
import { add } from "./math"        // relative path (your files)
import { Elysia } from "elysia"     // package from node_modules
```

Always include the `./` prefix for local files. Without it, the runtime looks in `node_modules`.
