# Actuator Health Groups and Kubernetes Probes

Spring Boot **Actuator** maps **health indicators** into **liveness** and **readiness** groups for **Kubernetes** without exposing every internal detail publicly.

---

## `management.endpoint.health.group.*`

Configure which indicators belong to **`readiness`** vs **`liveness`**—e.g. exclude **diskSpace** from readiness if not critical for accepting traffic.

---

## Probe endpoints

- **`/actuator/health/liveness`**
- **`/actuator/health/readiness`**

Wire **`Kubernetes` httpGet`** to these paths (and secure them appropriately).

---

## Startup probes

Long JVM + migration startup → **`startupProbe`** prevents kube from killing pods during warm-up.

---

## Related

- [Spring Boot Actuator](14-Spring-Boot-Actuator.md)
- [Spring Boot Availability Probes](70-Spring-Boot-Availability-Probes.md)
