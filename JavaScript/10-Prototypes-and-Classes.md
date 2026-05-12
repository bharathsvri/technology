# Prototypes and Classes

## Prototype chain

Objects delegate property lookup to `Object.getPrototypeOf(obj)`.

## `class` syntax (ES2015)

Syntactic sugar over prototypes:

```js
class Person {
  constructor(name) { this.name = name; }
  greet() { return `Hi ${this.name}`; }
}
```

## `extends` and `super`

Inheritance and parent constructor/method access.

## Static methods

`static` on class; belong to constructor, not instances.

Next: [ES Modules](11-ES-Modules-import-and-export.md).
