# Strict Mode and Temporal Dead Zone

## `'use strict'`

Enables stricter parsing and runtime errors (assigning undeleted globals, `delete` of plain names, etc.).

## Modules

ES modules are strict **automatically**.

## TDZ for `let`/`const`/`class`

From scope entry until initialization line executes, identifier is in **temporal dead zone**—access throws `ReferenceError`.

Next: [Timers](27-Timers-and-Scheduling.md).
