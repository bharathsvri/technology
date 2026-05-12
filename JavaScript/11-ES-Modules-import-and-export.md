# ES Modules: `import` and `export`

## Named exports

```js
export const PI = 3.14;
export function area(r) { return PI * r * r; }
```

## Default export

```js
export default function create() {}
```

## Import

```js
import create, { PI, area } from './math.js';
import * as math from './math.js';
```

## CORS and type

Browsers require `type="module"` on `<script>`; modules load **deferred** by default.

Next: [Arrays](12-Arrays-and-Array-Methods.md).
