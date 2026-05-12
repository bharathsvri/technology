# Custom Micrometer Metrics (`MeterRegistry`)

Beyond **auto-instrumentation**, apps register **business metrics**: counters, timers, gauges via **`MeterRegistry`** beans—surfaced in **Prometheus**, **OTLP**, etc.

---

## Patterns

- **`Counter.builder("orders.created").tag("region", region).register(registry).increment()`**
- **`Timer`** around service methods for **latency SLIs**.

---

## Naming conventions

Use **dot** notation and **low-cardinality** tags—**high cardinality** labels (user ids) explode **cardinality** in TSDBs.

---

## Testing

Use **`SimpleMeterRegistry`** in unit tests to assert **increment** counts.

---

## Related

- [Spring Boot Observability Micrometer OpenTelemetry](63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md)
- [Spring Boot Actuator](14-Spring-Boot-Actuator.md)
