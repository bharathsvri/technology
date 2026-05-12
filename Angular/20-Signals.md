# Signals

## Writable signal

```ts
count = signal(0);
increment() { this.count.update(c => c + 1); }
```

## Computed

`computed(() => this.count() * 2)` — memoized derived state.

## Effects

`effect(() => { console.log(this.count()); })` — side effects; watch signal reads.

## Interop with RxJS

`toSignal`, `toObservable` for hybrid codebases.

Next: [State management](21-State-Management-Patterns.md).
