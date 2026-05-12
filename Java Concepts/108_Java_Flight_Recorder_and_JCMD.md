# Java Flight Recorder (JFR) and jcmd

## Purpose
**JDK Flight Recorder (JFR)** records detailed runtime events (CPU, allocation, GC, locks, I/O) with low overhead. **`jcmd`** is the unified diagnostic command-line tool for a running JVM.

Together they answer: *Why is my service slow, allocating too much, or pausing during GC?*

## Prerequisites
- Use a **JDK** (not only a JRE) for full tooling.
- On containers/Kubernetes, ensure you can `kubectl exec` (or equivalent) into the pod as the same user or with rights to attach.

## Discover JVM Processes

```bash
jcmd
```

Output lists `<pid> <main class>` pairs.

## High-Value jcmd Commands

```bash
jcmd <pid> VM.version
jcmd <pid> VM.flags
jcmd <pid> GC.heap_info
jcmd <pid> Thread.print          # thread dump (similar to jstack)
jcmd <pid> VM.native_memory summary
```

## Record a JFR Profile (Continuous, Fixed Duration)

```bash
jcmd <pid> JFR.start name=profile settings=profile filename=/tmp/app.jfr duration=120s
```

- **`settings=profile`**: richer than `default`, still production-usable on modern JDKs.
- **`duration`**: auto-stop; omit for manual stop.

Stop and dump manually:

```bash
jcmd <pid> JFR.check
jcmd <pid> JFR.dump name=profile filename=/tmp/app.jfr
jcmd <pid> JFR.stop name=profile
```

## Analyze Recordings
- **JDK Mission Control (JMC)**: desktop tool to open `.jfr` files.
- Focus areas: **Hot Methods**, **Allocation**, **GC Pauses**, **Monitor/Lock**, **File I/O**.

## Flight Recorder API (Programmatic)
For controlled custom events, use the **`jdk.jfr`** package (define `@Label` events, commit from business code). Use sparingly in hot paths; prefer coarse business boundaries.

## Production Guidelines
- Start with **short captures** (1–5 minutes) during incidents.
- Prefer **`profile`** over **`default`** when you need allocation stacks, accepting slightly more overhead.
- Coordinate with security: recordings may include **string fields** and object samples that resemble sensitive data—treat files like secrets.

## Relationship to Other Tools
- **Micrometer / tracing**: request-level latency across services.
- **JFR**: deep JVM and library behavior inside one process.
- **Async Profiler / perf**: native CPU profiling; complements JFR.

## Quick Troubleshooting Map
- **Spiky latency**: GC + Monitor + Thread dumps.
- **High CPU**: Hot methods + compiler + thread states.
- **Memory growth**: Live objects / allocation profiling samples.
