

# What Is JavaScript?

A language created in 10 days in 1995 to make web pages interactive. Now it runs everywhere.

## The short history

> 🖼️ **[IMAGE_PLACEHOLDER]** — JavaScript history timeline 1995 to 2026

**1995** — Brendan Eich creates JavaScript at Netscape. Purpose: add interactivity to web pages. Name is a marketing trick (no relation to Java).

**1997** — ECMAScript becomes the standard. "ECMAScript" is the specification. "JavaScript" is the implementation. Everyone says JavaScript.

**2009** — Ryan Adams creates Node.js. JavaScript escapes the browser and runs on servers. One language for frontend and backend.

**2015** — ES6/ES2015. The biggest update in the language's history. `let`/`const`, arrow functions, classes, modules, promises, template literals. Modern JavaScript starts here.

**2012** — Microsoft releases TypeScript. A typed layer on top of JavaScript. Catches errors before running.

**2023** — Bun 1.0. A new JavaScript runtime built for speed. Replaces Node.js as the default for new projects.

## Why it is everywhere

The browser. Every device with a web browser can run JavaScript. No installation, no plugins, no setup. That made it the most widely deployed programming language.

Once JavaScript was in every browser, people built tools, frameworks, and ecosystems around it. Then Node.js let those same developers write server code. The same language, the same ecosystem, now covering the full stack.

## What JavaScript is

- **Interpreted / JIT compiled** — no build step, fast feedback loop
- **Single-threaded** — one thread of execution, but non-blocking I/O via the event loop
- **Dynamic typing** — variables can hold any type, types are checked at runtime
- **Multi-paradigm** — event-driven, functional, and object-oriented
- **Garbage collected** — you don't manage memory manually

## ECMAScript standard

JavaScript evolves through the ECMAScript specification (TC39 committee). New features go through stages. Modern runtimes (Bun, Deno, Node.js) implement the latest standard.

You will see references like "ES2023" or "ESNext." These refer to which version of the standard the feature belongs to. In practice, with Bun, you get the latest features immediately.
