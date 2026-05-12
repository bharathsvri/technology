# Spring Boot: Observability — Micrometer, Tracing, OpenTelemetry

Modern Spring Boot aligns metrics, tracing, and logs around **Micrometer** as a facade, with **OpenTelemetry (OTel)** as a common export path to vendors (Grafana Tempo, Jaeger, Datadog, New Relic, etc.).

## Three Pillars (Practical Mapping)
1. **Metrics**: JVM, HTTP server, JDBC pools, caches, custom timers/counters.
2. **Tracing**: distributed **traceId/spanId** across HTTP, messaging, JDBC (when instrumented).
3. **Logs**: correlate with trace context via **MDC** or structured JSON fields.

## Boot Starters (Conceptual)
Teams typically add:
- **`spring-boot-starter-actuator`** for endpoints and registry wiring.
- **Micrometer Observation** (`ObservationRegistry`) for unified handling of metrics + traces around an operation.
- **OTel Spring Boot starter** or **Micrometer Tracing Bridge** + OTLP exporter—verify current coordinates for your Spring Boot generation.

## Configuration Ideas (Illustrative YAML)
Properties evolve by version; treat these as patterns to look up in current docs:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  tracing:
    sampling:
      probability: 0.1   # example: sample 10% in high-traffic systems
  otlp:
    tracing:
      endpoint: http://otel-collector:4318/v1/traces
```

## `@Observed` and Custom Observations
Annotate important service boundaries or use `Observation.createNotStarted(...).observe(...)` to attach **low-cardinality** tags (tenant, feature flag name) while avoiding exploding cardinality (raw user IDs as tags are dangerous).

## Production Checklist (from community practice)
- Export **Prometheus** metrics from Actuator where applicable.
- Propagate **W3C Trace Context** headers on outbound calls (`RestClient`, `WebClient`, messaging templates).
- Align log fields with trace IDs (`traceId`, `spanId`).
- Validate overhead under load; adjust **sampling**.

## Related Topics
- `14-Spring-Boot-Actuator.md`
- `24-Spring-Boot-Logging.md`
- Java: `112_SLF4J_MDC_and_Structured_Logging.md`, `95_Monitoring_Observability.md`
