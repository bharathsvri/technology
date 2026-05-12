# Dynamic Components

## `ViewContainerRef.createComponent`

Imperatively instantiate components at runtime (modals, plugin UIs).

## `ComponentRef`

Access instance, inputs (`setInput` in recent versions), destroy on close.

## Limitations

Dynamic loading may require **standalone** or `NgModule` entryComponents legacy patterns removed in favor of direct imports.

Next: [Signals](20-Signals.md).
