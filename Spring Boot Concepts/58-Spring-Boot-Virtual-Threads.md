# Spring Boot: Virtual Threads (Project Loom)

## What Changes?
**Virtual threads** (Java 21+) are lightweight threads scheduled on carrier platform threads. For typical **blocking** Spring MVC + JDBC workloads, they can improve **throughput under load** with simpler code than reactive stacks.

## Enabling Virtual Threads (Spring Boot 3.2+)
Application property:

```yaml
spring:
  threads:
    virtual:
      enabled: true
```

Spring Boot maps this to Tomcat/Jetty/Undertow executor configuration so request handling uses virtual threads.

## Mental Model
- **Blocking I/O is OK** again at large scale—virtual threads yield instead of pinning a precious OS thread.
- You still must avoid **pinning** issues (very rare native blocking in synchronized blocks on early JDK versions—monitor progress notes for your exact JDK).

## What Virtual Threads Do *Not* Replace
- **CPU-bound** work still needs partitioning (fork/join, parallel streams with care, batch sizing).
- **Reactive** stacks (WebFlux) remain valuable for backpressure-heavy streaming endpoints.

## JDBC and Drivers
Use **recent JDBC drivers** known to be virtual-thread friendly; pool sizing semantics still matter (HikariCP).

## Observability
Ensure tracing/logging propagates context across continuations—validate your **Micrometer** / OpenTelemetry instrumentation version.

## When to Benchmark
- High concurrency, many blocking calls (HTTP client calls, DB queries).
- Compare against traditional thread pools with realistic load tests (latency percentiles, not only RPS).

## Related Docs
- [Virtual Threads (Java)](../Java%20Concepts/42_Virtual_Threads.md)
- [WebFlux](37-Spring-Boot-WebFlux.md) — compare programming models
