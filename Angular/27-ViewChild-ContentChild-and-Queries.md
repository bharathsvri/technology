# `@ViewChild`, `@ViewChildren`, `@ContentChild`, `@ContentChildren`

## View vs content

- **View** queries: elements **declared in this component’s template** (including from its template file).
- **Content** queries: nodes **projected into this component** via `<ng-content>` from the parent template.

## `@ViewChild` / `@ViewChildren`

```ts
@ViewChild('editor') editor?: ElementRef<HTMLTextAreaElement>;
@ViewChild(OrderFormComponent) form?: OrderFormComponent;
```

- **`{ static: true }`** (legacy): resolve before `ngOnInit` when needed in `ngOnInit`.
- Default **dynamic** timing: available after `ngAfterViewInit`.

Use **`read`** token to query directive instance vs `ElementRef`.

## `@ContentChild` / `@ContentChildren`

Query projected content—populated by **`ngAfterContentInit`**.

## Signal-based queries (modern Angular)

`viewChild`, `viewChildren`, `contentChild`, `contentChildren` as **signal queries**—preferred in new code for better typing and timing with `afterNextRender`.

Next: [DI advanced](28-Dependency-Injection-Advanced.md).
