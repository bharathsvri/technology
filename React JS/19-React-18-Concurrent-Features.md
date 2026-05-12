# React 18 Concurrent Features

## `createRoot`

Replaces `ReactDOM.render`; enables concurrent rendering.

## Automatic batching

Multiple `setState` calls in async handlers batch into one render.

## `startTransition`

Mark non-urgent updates so UI stays responsive.

## `useDeferredValue` / `useTransition`

Defer expensive rendering / keep previous UI while updating.

Next: [Strict Mode](20-Strict-Mode.md).
