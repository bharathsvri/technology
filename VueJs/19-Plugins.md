# Plugins

## Shape

```ts
const MyPlugin = {
  install(app) {
    app.config.globalProperties.$http = ...;
    app.provide('key', value);
  },
};
app.use(MyPlugin);
```

## Typing global properties

Augment `ComponentCustomProperties` in TypeScript.

Next: [Vite](20-Vite-and-Tooling.md).
