# Options API Overview

## Shape

```ts
export default {
  data() { return { count: 0 }; },
  computed: { double() { return this.count * 2; } },
  methods: { inc() { this.count++; } },
  mounted() { ... },
};
```

## When still used

Legacy codebases, tutorials; new code often prefers `<script setup>`.

## Mixins

Prefer **composables** (functions using Vue APIs) over mixins for reuse—clearer dependency graph.

Next: [Lifecycle](12-Lifecycle-Hooks.md).
