# API Idempotency Keys

**Idempotent** HTTP APIs return the **same effect** when retried with the same logical request—critical for **mobile flaky networks**, **gateway timeouts**, and **webhook** redelivery.

---

## Idempotency-Key header

Client sends **`Idempotency-Key: <uuid>`** on **POST** (or sensitive **PATCH**). Server:

1. Stores **(key, fingerprint, response snapshot)** in **Redis/DB** with **TTL**.
2. On duplicate key + same body → return **cached response** (same status/body).
3. On duplicate key + different body → **409 Conflict**.

---

## Spring Boot sketch

- **Filter** or **controller advice** reads header before hitting service.
- **Transactional** boundary around “check key + perform work + persist result”.
- Pair with **Problem Details** for errors (`62-Spring-Boot-Problem-Details-RFC9457.md`).

---

## Scope

- **Financial** APIs almost always require this.
- **GET/PUT/DELETE** can be naturally idempotent; **POST** usually is not.

---

## Related

- [Spring Boot REST APIs](06-Spring-Boot-REST-APIs.md)
- [Spring Boot Redis](29-Spring-Boot-Redis.md)
- [Spring Boot Rate Limiting](47-Spring-Boot-Rate-Limiting.md)
