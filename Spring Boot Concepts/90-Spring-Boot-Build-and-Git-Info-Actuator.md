# `spring.info` Build and Git Properties

Spring Boot can expose **`git.properties`** and **build-time metadata** via **`spring-boot-maven-plugin` / `gradle` build-info**—surfaced in **`/actuator/info`** for **release traceability**.

---

## Why it matters

- **Ops** correlates running artifact to **commit** and **CI build number**.
- **Support** asks “which version is prod?”—**info** endpoint answers without shell access.

---

## Security

**Sanitize** exposed keys—do not leak **internal** repo URLs or **secrets** masked poorly in properties files.

---

## Related

- [Spring Boot Actuator](14-Spring-Boot-Actuator.md)
- [Spring Boot SBOM Supply Chain](65-Spring-Boot-SBOM-Supply-Chain.md)
