# Angular CDK (Component Dev Kit)

## Common modules

- **Overlay** — floating panels, popovers, connected positioning.
- **Portal** — render UI logically elsewhere in the DOM (similar idea to React portals).
- **Virtual scroll** — recycle DOM nodes for huge lists (`cdk-virtual-scroll-viewport`).
- **Drag-drop** — sortable lists and drag handles with accessibility considerations.
- **Layout** — breakpoint observers (`BreakpointObserver`).
- **A11y** — focus trap, live announcer, high-contrast detection.

## Headless patterns

CDK gives **behavior** without Material visuals—useful for custom design systems.

## Tree-shaking

Import submodules you need; avoid blanket imports from umbrella packages when possible.

Next: [SSR and Angular Universal](33-SSR-Angular-Universal.md).
