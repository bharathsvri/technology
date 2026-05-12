# GraalVM Native Image (Java)

## What Is Native Image?
**Native Image** ahead-of-time (AOT) compiles Java bytecode into a **standalone native executable**. Startup time and memory footprint are often much lower than a traditional JVM, at the cost of **longer build times** and some **dynamic Java features** requiring explicit configuration.

## Typical Use Cases
- **CLI tools** and batch jobs where fast cold start matters.
- **Serverless** functions with tight startup SLOs.
- **Kubernetes** microservices when memory density is critical (evaluate per workload).

## How It Differs from the JVM
- **No JIT warmup** in the same way; peak throughput may differ from HotSpot on some workloads.
- **Classpath closed-world**: reflection, JNI, resource loading, and classpath scanning must be declared if not discoverable statically.
- **Build-time initialization** options trade compatibility for performance.

## Spring Boot Integration (Conceptual)
Spring Boot 3+ provides GraalVM Native Image support via:
- **`spring-boot-starter-parent`** + native build tools plugin (Maven or Gradle).
- **`spring-aot`** processing that generates reachability metadata for Spring’s dynamic behavior.

Typical build (Gradle):

```bash
./gradlew nativeCompile
```

Maven uses an analogous native profile/plugin coordinate.

## Common Metadata Needs
When something fails at runtime with `ClassNotFoundException` or missing resource:
- **`reflect-config.json`**, **`resource-config.json`**, **`serialization-config.json`** (or generated equivalents).
- Spring often auto-generates these; third-party libraries may need hints.

## Trade-offs Checklist
- **Pros**: fast startup, smaller RSS in many cases, simpler deployment artifact (single binary).
- **Cons**: build complexity, platform-specific binaries, constrained reflection/proxy patterns unless configured.

## Operational Notes
- Prefer **musl vs glibc** awareness when targeting minimal Linux images.
- Observability: ensure your **OpenTelemetry** / logging appenders support native image or supply reachability metadata.

## When Not to Use Native Image
- Heavy dynamic bytecode generation, exotic agents, or libraries incompatible with closed-world assumptions without significant tuning.

## Further Reading
- GraalVM Native Image compatibility guide.
- Spring Boot reference: *GraalVM Native Image Support*.
