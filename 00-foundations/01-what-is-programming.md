

# What Is Programming?

A program is a set of instructions a computer follows, step by step.

Think of a recipe:

1. Boil water
2. Add pasta
3. Wait 8 minutes
4. Drain
5. Serve

A recipe has inputs (ingredients), steps (instructions), and an output (cooked pasta). Code works the same way.

## A program has three things

**Input** — data the program receives. A user typing their name. A file being uploaded. A database row.

**Processing** — what the program does with that input. Calculating a total. Filtering results. Transforming data.

**Output** — the result. A response sent back. A file saved. A row inserted.

## Code is just instructions

```typescript
const price = 10
const tax = 0.08
const total = price + price * tax

console.log(total) // 10.8
```

Line by line: store 10 in `price`, store the tax rate, calculate the total, print it.

## Why "write code" and not "use a tool"?

Because tools solve specific problems. Code solves any problem that can be broken into steps.

Need to process 10,000 records? Calculate shipping for 50 countries? Validate a form with 20 rules? A GUI tool can't handle that. Code can.

## Computers are literal

Computers do exactly what you tell them. Nothing more, nothing less. If you say "add salt" but meant "add sugar," you get salty cake. There is no "you know what I mean."

This is why programming is precise. This is also why bugs exist — the computer followed your instructions, but your instructions were wrong.

## The good news

Modern languages like TypeScript catch many mistakes before you run the code. If you try to multiply a string, TypeScript says "that won't work" before the program ever runs.

That is the entire point of this learning path: write instructions that work, catch mistakes early, and build things people use.
