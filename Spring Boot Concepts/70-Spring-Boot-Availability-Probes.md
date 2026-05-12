# Spring Boot: Liveness, Readiness, and Availability State

Kubernetes (and similar orchestrators) use **HTTP probes** to decide when a pod is **live** (should be restarted) vs **ready** (should receive traffic). Spring Boot exposes these through **Actuator** with clear separation from generic “up” health.

## Key Ideas
- **Liveness**: the process is **making progress**; if this fails, the platform may restart the container.
- **Readiness**: the app can **safely serve traffic** (database reachable, migrations done, warm-up complete).
- **Startup** (Kubernetes 1.16+): long initialization without marking unready forever.

## Boot Configuration (Patterns)
Property names evolved; on modern Boot 3.x you commonly see:

```yaml
management:
  endpoint:
    health:
      probes:
        enabled: true
  health:
    livenessstate:
      enabled: true
    readinessstate:
      enabled: true
```

Expose the **Actuator health** endpoint on a **management port** or path your platform scrapes.

## Application Code Integration
Use **`AvailabilityChangeEvent.publish`** to move readiness when async initialization completes (cache warmed, leader election done).

## Common Mistakes
- Putting **expensive checks** in liveness—causes restart loops.
- Readiness tied to **non-critical** fluff—removes capacity unnecessarily.

## Related Topics
- `14-Spring-Boot-Actuator.md`
- `51-Spring-Boot-Health-Indicators.md`
- `61-Spring-Boot-Graceful-Shutdown.md`
