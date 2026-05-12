# React Router (v6+)

## `BrowserRouter` + `Routes`

```tsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/users/:id" element={<User />} />
</Routes>
```

## Navigation

`<Link to="...">`, `useNavigate()`, `useParams()`, `useSearchParams()`.

## Loaders (Remix-style in RR data APIs)

Fetch data before render when using data router APIs.

Next: [Data fetching](12-Data-Fetching-Patterns.md).
