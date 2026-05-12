# Creating an Application

## Vue 3

```ts
import { createApp } from 'vue';
import App from './App.vue';
createApp(App).mount('#app');
```

## Plugins

```ts
createApp(App).use(router).use(pinia).mount('#app');
```

## SFC compile

`@vitejs/plugin-vue` transforms `.vue` files at build time.

Next: [Template syntax](03-Template-Syntax-and-Directives.md).
