# Structured Concurrency (Java 21+)

**Structured concurrency** treats groups of related tasks as a **single unit of work**: subtasks have a clear **scope**, **lifetime**, and **failure policy** tied to their parent. In modern JDKs this centers on **`java.util.concurrent.StructuredTaskScope`** (finalized in **Java 21** alongside virtual threads).

## Problem It Addresses
Fire-and-forget `ExecutorService.submit` calls are hard to reason about:
- Which subtasks belong together?
- If one fails, what happens to siblings?
- Shutdown ordering is manual and error-prone.

## StructuredTaskScope (Java 21+)

```java
import java.util.concurrent.StructuredTaskScope;

try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    StructuredTaskScope.Subtask<String> user = scope.fork(this::fetchUser);
    StructuredTaskScope.Subtask<Integer> orders = scope.fork(this::fetchOrderCount);
    scope.join();
    scope.throwIfFailed();
    return new Dashboard(user.get(), orders.get());
}
```

## Policies (Conceptual)
- **ShutdownOnFailure**: cancel remaining work if one subtask fails—good for “all or nothing” fan-out.
- **ShutdownOnSuccess**: racing redundant providers (first good result wins).

## Relationship to Virtual Threads
Structured concurrency pairs naturally with **virtual threads** for I/O-bound fan-out without thread-pool explosion—still define **timeouts** and **cancellation** policies.

## Pitfalls
- Blocking inside `fork` without timeouts can stall the whole scope.
- Mixing with legacy thread pools requires clear ownership of lifetimes.

## Related Topics
- `42_Virtual_Threads.md`
- `35_Advanced_Multithreading.md`
- Spring Boot: `58-Spring-Boot-Virtual-Threads.md`
