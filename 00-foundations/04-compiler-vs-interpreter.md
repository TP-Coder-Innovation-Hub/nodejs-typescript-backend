

# Compiler vs Interpreter

Computers only understand machine code (binary). Your TypeScript code is not machine code. Something has to translate it.

## Two approaches

**Compiler** — translates the entire program to machine code before it runs. C, C++, Go, Rust work this way. You compile once, run the result many times.

**Interpreter** — reads and executes code line by line, at runtime. Python, Ruby, and traditional JavaScript work this way.

## JavaScript is interpreted (with a twist)

JavaScript was originally a pure interpreter. The browser read each line and executed it.

Modern JavaScript engines (V8 in Chrome/Bun, SpiderMonkey in Firefox) use a hybrid approach called JIT (Just-In-Time) compilation:

1. Code is parsed into an abstract syntax tree
2. The interpreter starts executing immediately
3. Code that runs frequently gets compiled to optimized machine code on the fly
4. The engine replaces slow interpreted code with fast compiled code

You get the fast startup of an interpreter and the speed of compiled code for hot paths.

## What this means for you

**No build step.** You write code and run it. With Bun:

```typescript
console.log("hello")
```

```bash
bun run index.ts
```

No compile command. No binary to produce first. The runtime handles everything.

**Errors show up at runtime.** A compiled language like Go catches type errors during compilation. JavaScript does not — it discovers errors when the bad line executes.

TypeScript fixes this. TypeScript is a compiler that checks your code before it runs. But TypeScript compiles to JavaScript, and the JavaScript runs in an interpreter (or JIT).

```
TypeScript source → (type check) → JavaScript → (JIT) → machine code → runs
```

## Why does this matter for backend?

Speed of iteration. You change a file, save, and the server reloads. No 30-second compile step. This is why JavaScript dominates web development — the feedback loop is fast.

The tradeoff: you don't catch all errors before running. TypeScript catches type errors, but not logic errors. That is why testing matters. We will cover that later.
