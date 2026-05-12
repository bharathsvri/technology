# JavaScript Introduction and History

## What is JavaScript?

**JavaScript** (standardized as **ECMAScript**) is a **dynamic**, **garbage-collected** language that powers browsers and servers (**Node.js**, **Deno**, **Bun**). It supports **multiple paradigms**: procedural, object-oriented (prototypes and classes), and **functional** (first-class functions, closures).

---

## A short history (why the language looks the way it does)

| Era | Highlights |
|-----|--------------|
| **1995** | Created for Netscape Navigator in ~10 days — quick iteration left quirks (`typeof null === 'object'`). |
| **ES3 / ES5** | Widely deployed baseline; `strict mode`, JSON, array methods. |
| **ES2015 (ES6)** | `let`/`const`, classes, modules, promises, arrow functions — **biggest** single jump. |
| **Yearly releases** | ES2016+ adds `async/await`, optional chaining, `BigInt`, top-level `await`, etc. |

Engines (**V8**, **SpiderMonkey**, **JavaScriptCore**) compete on speed; TC39 committee governs the **language spec**.

---

## Where JavaScript runs

1. **Browser** — DOM, `fetch`, `localStorage`, Web APIs; single **main thread** + **Web Workers**.
2. **Node.js** — file system, TCP/HTTP servers, native addons; **CommonJS** + **ESM** coexist.
3. **Embedded** — IoT, game scripting, tooling (esbuild, ESLint).

---

## Spec vs engines vs browsers

- **ECMA-262** — the language definition.
- **MDN** — practical reference for Web APIs + JS.
- **Can I use** — browser support matrices for Web APIs (not core syntax only).

---

## Strict mode and modules

`'use strict'` tightens rules; **ES modules** are strict **automatically**. Prefer modules for new apps.

---

## How to read the rest of this folder

1. **Values & types** → variables, coercion, operators.
2. **Functions & scope** → closures, `this`, arrows.
3. **Objects & modules** → classes, `import`/`export`.
4. **Async** → Promises, `async`/`await`.
5. **Browser** → DOM, events, `fetch`, storage.
6. **Tooling** → Node/npm, performance, debugging.

---

## Next

- [Variables: let, const, and var](02-Variables-let-const-and-var.md)
