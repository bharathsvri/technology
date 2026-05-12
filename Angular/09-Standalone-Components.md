# Standalone Components and `bootstrapApplication`

## Standalone component

```ts
@Component({
  standalone: true,
  imports: [RouterOutlet, HeaderComponent],
  selector: 'app-root',
  template: `<app-header /><router-outlet />`,
})
export class AppComponent {}
```

## Bootstrap

```ts
// main.ts
bootstrapApplication(AppComponent, {
  providers: [provideRouter(routes), provideHttpClient()],
});
```

## `import` in decorator

Pull in other standalone components, directives, pipes, and module compatibility imports.

Next: [Routing](10-Routing-and-Navigation.md).
