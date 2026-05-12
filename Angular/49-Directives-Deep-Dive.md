# Directives — Deep Dive

## Structural directive microsyntax

`*ngIf="expr; else tpl"` expands to `<ng-template>` sugar—understanding the microsyntax helps debug template errors.

## Attribute directives

Lightweight DOM enhancements: `pHighlight`, tooltips, permissions hiding.

## `exportAs`

Expose directive instance to template variables: `#dir="exportName"`.

## `standalone: true` directives

Import directly into component `imports` array.

## Performance

Structural directives **recreate** DOM subtrees—prefer **`@for` with track** or stable `*ngFor` `trackBy` for large lists.

Next: [Zone.js and zoneless change detection](50-Zone-js-and-Zoneless.md).
