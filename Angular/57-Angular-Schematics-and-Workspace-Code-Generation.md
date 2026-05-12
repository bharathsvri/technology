# Angular Schematics and Workspace Code Generation

**Schematics** are **AST-aware** transforms used by **`ng generate`**, migrations, and **custom** enterprise blueprints—distinct from simple file templates.

---

## What schematics do

- **Add** / **update** / **move** files in a **Tree** (virtual file system).
- Apply **rules** and **chains**; can prompt for options (`schema.json`).
- Run as part of **`ng update`** migrations (e.g. standalone migration).

---

## When teams write custom schematics

- **Standardize** feature modules, CRUD screens, or **Ngrx** slices.
- **Enforce** lintable folder layout and **barrel** imports.

---

## vs snippets / copilot

Schematics guarantee **repo-wide consistency** and can update **existing** code (migrations), not only insert boilerplate.

---

## Interview soundbites

- **RuleFactory** returns a **Rule**; **`chain`** composes steps.
- **Testing** uses **UnitTestTree** to assert virtual file outputs.

---

## Related

- [Angular CLI](22-Angular-CLI.md)
- [Workspace and Libraries](40-Workspace-and-Libraries.md)
