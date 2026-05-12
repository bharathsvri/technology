# TypeScript Essentials for Angular

## Types on inputs/outputs

```ts
@Input() userId!: string;
@Output() saved = new EventEmitter<void>();
```

## Interfaces for models

```ts
export interface User { id: string; name: string; }
```

## Strict mode

Enable `strict`, `strictTemplates` in `tsconfig` for safer templates.

## Decorators

`@Component`, `@Injectable`, etc. rely on TS **experimentalDecorators** / new standard decorators per Angular version.

Next: [Components](04-Components-and-Templates.md).
