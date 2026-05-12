# Spring Boot: Graceful Shutdown and Lifecycle

## Why Graceful Shutdown Matters
During **Kubernetes rollouts** or VM restarts, in-flight HTTP requests and message consumers should finish (within a budget) instead of being cut mid-transaction.

## Server Graceful Shutdown (Servlet Stack)
Spring Boot embeds Tomcat/Jetty/Undertow with hooks to stop accepting new work, drain existing requests, then exit.

Typical properties:

```yaml
server:
  shutdown: graceful

spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s
```

Tune timeouts to your **longest legitimate** request plus buffer.

## Connection Pools and Brokers
Shutdown order should close:
- **Kafka/Rabbit consumers** (stop polling, commit offsets).
- **HTTP clients** and **DB pools** (drain / close).
- **Schedulers** (`@Scheduled`)—pause or let tasks complete based on idempotency.

Use **`SmartLifecycle`** or **`@PreDestroy`** beans with explicit phases where ordering matters.

## Kubernetes Probes Coordination
- **`terminationGracePeriodSeconds`** must be **≥** application drain timeout + JVM exit time.
- Keep **`readiness`** accurate so removed pods stop receiving new traffic early in a rollout.

## Common Pitfalls
- Background **virtual threads / executors** not shut down—JVM hangs.
- **Non-daemon** threads preventing process exit.
- Overly long drains blocking deployments—balance UX with SLOs.

## Related Docs
- `14-Spring-Boot-Actuator.md` – health during shutdown transitions.
- `40-Spring-Boot-Performance-Optimization.md` – pool sizing context.
