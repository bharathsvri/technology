# Spring Boot: JAR vs WAR deployment

## Default: executable JAR
Spring Boot’s usual packaging is an **executable “fat” JAR** embedding the server (Tomcat, Jetty, or Undertow). One artifact, one process model—ideal for **containers** and **Kubernetes** (`16-Spring-Boot-Docker.md`).

## When a WAR still appears
- **Traditional app servers** (WebLogic, WebSphere, JBoss EAP) mandate a **WAR** deployed to a shared container.
- **Multiple apps** sharing a hardened container image maintained by a platform team.

## Boot changes in WAR mode
- Your application class typically **`extends SpringBootServletInitializer`** and overrides **`configure`** so the external container can bootstrap Spring.
- Mark embedded server dependencies as **`provided`** scope so they do not conflict with the container’s servlet implementation (exact POM snippets depend on your stack).

## Trade-offs
| Topic | Executable JAR | External-container WAR |
|-------|----------------|-------------------------|
| Operations | Single artifact; aligns with **12-factor** | Fits legacy **change boards** for shared middleware |
| Class loaders | Simpler | Container rules, **shared libraries**, TLD scanning quirks |
| Observability | You own the whole JVM | May require extra agent wiring per vendor |

## Related docs
- `01-Introduction-to-Spring-Boot.md` — mental model
- `61-Spring-Boot-Graceful-Shutdown.md` — signal handling in containers
- `82-Spring-Boot-Native-Image-GraalVM-Overview.md` — native images are JAR-first today
