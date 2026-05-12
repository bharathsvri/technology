# Outbox Pattern for Reliable Messaging

The **transactional outbox** ensures **domain data** and **“message to send”** commit **atomically** in the database, then a separate **relay** publishes to Kafka/RabbitMQ—avoiding **dual-write** inconsistency.

---

## Problem it solves

If you **write DB** then **publish event**, a crash between steps → **lost event** or duplicate handling without extra care.

---

## Pattern

1. In the same DB transaction as business writes, **insert a row** into **`outbox`** (payload, type, correlation id).
2. **Publisher process** (polling or log-tailing) reads pending rows, publishes to broker, marks **sent** or deletes.
3. Consumers must be **idempotent** (at-least-once delivery).

---

## Spring Boot implementation sketch

- **JPA entity** `OutboxEvent` + **repository**.
- **`@Transactional`** service writes domain + outbox row.
- **`@Scheduled`** or **Debezium CDC** drives relay (CDC avoids polling latency).

---

## Trade-offs

| Pros | Cons |
|------|------|
| Strong consistency with DB state | Extra table + operational **relay** |
| Works with any broker | Ordering guarantees need **partition key** design |

---

## Related

- [Spring Boot Kafka](38-Spring-Boot-Kafka.md)
- [Spring Boot RabbitMQ](31-Spring-Boot-RabbitMQ.md)
- [Spring Boot Transactions](10-Spring-Boot-Transactions.md)
- [Spring Boot Scheduling](15-Spring-Boot-Scheduling.md)
