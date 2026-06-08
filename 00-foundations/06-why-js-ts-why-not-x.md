

# Why JavaScript/TypeScript? Why Not X?

Language choice is about tradeoffs. Here is when JS/TS wins, and when it does not.

## When to choose JS/TS

**You are building for the web.** Browsers run JavaScript. Full stop. If your product is a web app, at minimum your frontend is JavaScript. Using JS/TS on the backend means one language across the stack. One set of tools, one hiring pool, one mental model.

**Speed of development matters more than raw performance.** JS/TS has the fastest feedback loop of any backend language. No compile step. Hot reload. Change a line, see the result. For startups, prototypes, and features that need to ship fast, this is the deciding factor.

**You want the largest ecosystem.** npm has over 2 million packages. Whatever you need to do, someone has already written a library for it. More developers know JavaScript than any other language.

**You want type safety without the overhead.** TypeScript gives you compile-time type checking with optional annotations. You get the safety of a typed language without the ceremony of Java or the strictness of Haskell.

## When to choose something else

**Python** — ML/AI, data science, scientific computing. Python's ecosystem (NumPy, Pandas, PyTorch) dominates these fields. If you are building ML pipelines, use Python.

**Go** — High-throughput services, infrastructure tooling, CLIs. Go compiles to a single binary. Excellent concurrency with goroutines. If you need raw throughput and simple deployment, Go is the better choice.

**Java** — Enterprise systems, Android, large teams with existing Java investment. The JVM ecosystem is mature. If your company already runs Java, adding Node.js creates fragmentation.

**Rust** — Systems programming, performance-critical code, WebAssembly. Rust gives you zero-cost abstractions and memory safety without garbage collection. If you are building a database engine or a browser, use Rust.

## The practical answer

For most web backends — APIs, CRUD apps, real-time services — JS/TS is the right choice. The ecosystem is unmatched, the developer pool is deep, and with Bun + TypeScript, the DX is excellent.

For specialized domains (ML, systems, high-throughput infra), use the right tool. But know that you will reach for JS/TS more often than you reach for anything else.
