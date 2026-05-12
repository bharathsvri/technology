# Workspace, Libraries, and Monorepo

## Angular workspace

Single repo with **`angular.json`** `projects` map: **applications** and **libraries**.

## Library project

`ng generate library my-ui` — publishable package or internal shared UI.

## Path mappings

`tsconfig` **paths** map `@myorg/ui` → `dist/my-ui` or source for local dev.

## Versioning

Use **semantic versioning** for published libs; **changelog** per library.

## Nx (optional ecosystem)

Many teams pair **Nx** with Angular for **affected** builds, caching, and enforced module boundaries—conceptually similar goals to Spring Modulith.

Next: [NgModule to standalone migration](41-Migrating-NgModule-to-Standalone.md).
