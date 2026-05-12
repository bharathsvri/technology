# Routing and Navigation

## `provideRouter`

```ts
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users/:id', component: UserDetailComponent },
  { path: '**', redirectTo: '' },
];
```

## Router outlet

`<router-outlet />` renders matched route component.

## Navigation

`Router.navigate`, `routerLink`, `routerLinkActive`.

## Route data and params

`ActivatedRoute.snapshot` vs **observable** `paramMap`, `data` for reactive updates.

Next: [Guards](11-Route-Guards-and-Resolvers.md).
