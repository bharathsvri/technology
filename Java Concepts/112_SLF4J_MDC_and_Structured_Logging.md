# SLF4J MDC and Structured Logging

## Why This Matters
In distributed systems, logs must be **correlatable** across threads and services. **Mapped Diagnostic Context (MDC)** attaches key-value metadata (for example `traceId`, `userId`) to every log line emitted on that thread.

## SLF4J MDC Basics

```java
import org.slf4j.MDC;

MDC.put("traceId", traceId);
try {
    log.info("Processing payment");
} finally {
    MDC.remove("traceId"); // avoid leaks in thread pools
}
```

### Thread Pools and Leaks
Worker threads are reused. Always **`remove`** keys you set, or clear in a `finally` block / task wrapper, otherwise stale IDs appear on unrelated requests.

## Logback Pattern Example
In `logback-spring.xml`, include MDC keys:

```xml
<pattern>%d{ISO8601} [%thread] %-5level %logger - traceId=%X{traceId} - %msg%n</pattern>
```

## Virtual Threads Note (Java 21+)
MDC is typically **thread-local**. When work jumps across virtual threads or structured tasks, ensure context propagation matches your logging provider version and any **reactive** pipeline (Reactor has `Context`, Micrometer Tracing bridges differ).

## Structured Logging (JSON)
Emitting JSON logs (via Logback encoder or Log4j2 JSON layout) helps observability stacks parse fields:
- `timestamp`, `level`, `logger`, `message`
- `traceId`, `spanId` (when using Micrometer Tracing / OpenTelemetry)

## Best Practices
- Keep MDC **small** (IDs and coarse labels), not large payloads.
- Populate trace identifiers as early as possible (servlet filter, WebFlux WebFilter, messaging consumer interceptor).
- Align field names with your **tracing** tool so logs and traces join cleanly.

## Related Topics
- `45_Logging.md` – general logging APIs.
- `95_Monitoring_Observability.md` – metrics and tracing.
