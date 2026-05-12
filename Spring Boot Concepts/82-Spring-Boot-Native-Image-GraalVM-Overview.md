# Spring Boot Native Images (GraalVM) — Conceptual Overview

**Spring Native / GraalVM native image** compiles a **closed-world** application to a **binary** with **fast startup** and **lower RSS**—ideal for **serverless** and **CLI**, with **trade-offs** in reflection and **build time**.

---

## Why it is different from JVM

Native image performs **ahead-of-time** analysis; **dynamic classpath features** (unchecked reflection, some proxies) need **hints** (`reflect-config.json`, **Reachability Metadata**).

---

## Spring Boot 3 alignment

Use official **GraalVM Native Build Tools** plugin; **test** native image in CI (build minutes are **long**).

---

## Interview trade-offs

| Pros | Cons |
|------|------|
| Fast cold start | Long **compile** times |
| Smaller attack surface (no JIT) | **Dynamic** features need configuration |
| Good for containers at scale | Some libraries **unsupported** |

---

## Related

- [GraalVM Native Image Java](../Java%20Concepts/109_GraalVM_Native_Image_Java.md)
- [Spring Boot Performance Optimization](40-Spring-Boot-Performance-Optimization.md)
