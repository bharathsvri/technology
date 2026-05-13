# Spring Boot: Spring Session (Distributed Sessions)

## Problem
Default servlet sessions are **in-memory** in the application process. After horizontal scaling or rolling deploys, users can lose state unless you use **sticky load balancing** (operationally brittle) or a **shared session store**.

**Spring Session** abstracts session persistence behind the same `HttpSession` API while backing storage with **Redis**, **JDBC**, **MongoDB**, or other modules.

## When You Still Need Server-Side Sessions
- **Browser sessions** authenticated with **cookies** and traditional server-side login flows.
- **OAuth2 login** flows that stash transient state server-side (always harden with CSRF and cookie flags — `78-Spring-Boot-CORS-CSRF-and-Browser-Security.md`).

For **stateless APIs** (Bearer tokens, mTLS), prefer **no server session**; this doc targets cookie-oriented or hybrid apps.

## Spring Boot Configuration (Conceptual)
- Set **`spring.session.store-type`** to `redis`, `jdbc`, `mongo`, etc. (exact enum values follow your Boot version’s reference).
- Provide the corresponding datastore connection (for Redis see also `29-Spring-Boot-Redis.md` for cache and pub/sub concerns — isolate **session key prefixes** from cache keys).

## Redis Example (Idea)
- Add **`spring-session-data-redis`** (coordinates via BOM).
- Configure **connection**, **timeouts**, and optional **namespace** so multiple apps sharing a cluster do not collide.
- Modern Boot often auto-configures Spring Session when the module and store are present; prefer property-driven setup over legacy `@EnableRedisHttpSession` unless you have a specific reason.

## JDBC Store
Useful when Redis is unavailable but you already operate a **relational HA cluster**. Requires **schema initialization** (SQL scripts provided by Spring Session) and attention to **cleanup** / **expiry** jobs for stale sessions.

## Security Checklist
- **Cookie flags**: `Secure`, `HttpOnly`, sensible **`SameSite`** for your deployment topology.
- **Session fixation**: Spring Security integrates with session management strategies; review defaults when you introduce custom login pages.
- **Transport**: terminate TLS at the edge or in-cluster consistently (`73-Spring-Boot-SSL-Bundles.md`).

## Observability
Session store latency affects every interactive request. Watch pool usage, command timeouts, and errors alongside HTTP metrics (`63-Spring-Boot-Observability-Micrometer-OpenTelemetry.md`).

## Related Docs
- `12-Spring-Boot-Security.md`, `57-Spring-Boot-OAuth2-and-Spring-Security.md` — authentication models.
- `29-Spring-Boot-Redis.md` — Redis fundamentals beyond sessions.
- `40-Spring-Boot-Performance-Optimization.md` — session size and serialization costs.
