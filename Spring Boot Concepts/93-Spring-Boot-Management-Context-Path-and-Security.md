# Management Endpoints Context Path and Security

Spring Boot Actuator endpoints can live on a **separate** **`management.server.port`** or **`management.endpoints.web.base-path`**—security rules must **explicitly** cover **`/actuator/**`** vs application **`context-path`**.

---

## Split management port

**`management.server.port=9090`** isolates **metrics** from public **8080**—common in **Kubernetes** network policies.

---

## Security filter chain (Spring Security 6)

Order **security matcher** for **`/actuator/health`** vs authenticated **`/actuator/prometheus`** carefully.

---

## Pitfalls

- **Double `context-path`** confusion when reverse proxy strips prefixes—align **`server.forward-headers-strategy`**.

---

## Related

- [Spring Boot Actuator](14-Spring-Boot-Actuator.md)
- [Spring Boot Security](12-Spring-Boot-Security.md)
