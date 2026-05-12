# Node.js and npm Overview

## Node

V8 + libuv: **non-blocking I/O**, `fs`, `http`, `path`, `crypto`, streams.

## npm / yarn / pnpm

Package registries; **`package.json`** scripts (`start`, `test`, `build`); lockfiles for reproducible installs.

## CommonJS vs ESM

Node supports both `require`/`module.exports` and `import`/`export` (per `package.json` `"type": "module"`).

## Minimal HTTP server

```js
import http from 'node:http';
http.createServer((req, res) => res.end('ok')).listen(3000);
```

Next: [Dynamic import](31-Dynamic-import.md).
