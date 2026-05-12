# `RouteReuseStrategy` and Preloading

## `RouteReuseStrategy`

Controls whether Angular **destroys** route components when navigating away—default destroys. Custom strategies can **cache** component instances (e.g. tabbed master-detail) but add **memory** and **stale state** risks.

## Implement cautiously

Return `shouldDetach` / `shouldAttach` / `retrieve` / `store` with clear keys based on route configs.

## Preloading lazy routes

`provideRouter` with **`withPreloading(PreloadAllModules)`** or a **custom `PreloadingStrategy`** that preloads after idle or based on user role.

## `loadComponent` prefetch

Combine with intersection observers or hover intents for smarter prefetch than “everything”.

Next: [`APP_INITIALIZER`](38-APP-Initializer-and-Bootstrap.md).
