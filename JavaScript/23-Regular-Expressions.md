# Regular Expressions

## Literals and constructor

```js
/\d+/g
new RegExp('\\d+', 'g')
```

## Common flags

`g` global, `i` ignore case, `m` multiline, `s` dotAll, `u` unicode.

## Methods

`test`, `exec`, `String.prototype.match`, `matchAll`, `replace`, `split`.

## Performance

Avoid catastrophic backtracking on user-controlled patterns.

Next: [Map and Set](24-Map-Set-WeakMap-WeakSet.md).
