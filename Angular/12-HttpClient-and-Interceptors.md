# HttpClient and Interceptors

## `HttpClient`

```ts
this.http.get<User[]>('/api/users');
```

Typed responses, `observe: 'response'` for headers/status.

## Interceptors

`HttpInterceptorFn` — add auth headers, logging, retry, error mapping.

```ts
export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const clone = req.clone({ setHeaders: { Authorization: `Bearer ${token}` } });
  return next(clone);
};
```

Register with `provideHttpClient(withInterceptors([authInterceptor]))`.

Next: [Template-driven forms](13-Template-Driven-Forms.md).
