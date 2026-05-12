# Pipes

## Built-in

`date`, `currency`, `decimal`, `percent`, `uppercase`, `json`, `async` (subscribes to `Observable`/`Promise`).

## Pure vs impure

Pure pipes memoize on input reference change; impure run every CD cycle—expensive if misused.

## Custom pipe

```ts
@Pipe({ name: 'highlight', standalone: true })
export class HighlightPipe implements PipeTransform {
  transform(value: string, q: string) { ... }
}
```

Next: [Services and DI](08-Services-and-Dependency-Injection.md).
