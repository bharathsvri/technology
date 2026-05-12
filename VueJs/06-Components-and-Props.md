# Components and Props

## Define props

`<script setup lang="ts">` with `defineProps<{ title: string }>()` or runtime array form.

## One-way data flow

Mutating props in child is anti-pattern—emit events or use v-model pattern.

## Prop validation

Runtime `type`, `required`, `default` in Options API or `withDefaults` in TS.

Next: [Emits and v-model](07-Emits-and-v-model.md).
