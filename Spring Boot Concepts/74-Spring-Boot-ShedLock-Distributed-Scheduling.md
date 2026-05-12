# Spring Boot: Distributed Scheduling with ShedLock

`@Scheduled` in Spring runs **in every instance** of your service by default—dangerous for cron-style jobs that must run **at most once** across a cluster.

**ShedLock** ensures only one node executes a scheduled task at a time using a **shared lock store** (JDBC, Redis, MongoDB, Zookeeper, etc.).

## Concept
1. Task starts → tries to **acquire** a named lock with a **lockAtMostFor** duration.
2. If acquisition fails, other instances skip this run.
3. If the instance dies mid-run, the lock **expires** so another node can take over after `lockAtMostFor`.

## Dependency (Illustrative)
Add **`shedlock-spring-boot-starter`** plus a provider (e.g., JDBC template provider, Redis provider)—verify coordinates for your Boot major version.

## Sketch

```java
import net.javacrumbs.shedlock.spring.annotation.SchedulerLock;
import org.springframework.scheduling.annotation.Scheduled;

@Scheduled(cron = "0 0 * * * *")
@SchedulerLock(name = "nightlyReport", lockAtMostFor = "PT50M", lockAtLeastFor = "PT1M")
public void nightlyReport() {
    // cluster-safe single execution
}
```

## Choosing Durations
- **`lockAtLeastFor`**: prevents rapid re-entry if the job finishes too quickly (optional).
- **`lockAtMostFor`**: must exceed worst-case runtime but not be so long that a dead node blocks recovery forever.

## Alternatives
- **Leader election** (Kubernetes lease, Zookeeper curator).
- **External scheduler** (Airflow, Cloud Scheduler) calling an HTTP endpoint once.

## Related Topics
- `15-Spring-Boot-Scheduling.md`
- `29-Spring-Boot-Redis.md` (Redis-backed lock provider)
