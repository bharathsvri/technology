# Migrating from `NgModule` to Standalone

## Motivation

Fewer boilerplate files, clearer **import graph** per component, better tree-shaking ergonomics in modern Angular.

## Steps (high level)

1. Add **`standalone: true`** to components/directives/pipes.
2. Move `imports` / `providers` from `NgModule` into each standalone root or **`ApplicationConfig`** providers.
3. Replace `RouterModule.forRoot` with **`provideRouter`**.
4. Remove empty `NgModule` classes once nothing references them.

## Schematics

`ng generate @angular/core:standalone` migration schematic (availability depends on Angular version—run `ng update` first).

## Hybrid period

Standalone components can still **import** legacy modules via **`importProvidersFrom`** for third-party libs not yet standalone.

Next: [Jest / Vitest testing](42-Testing-with-Jest-or-Vitest.md).
