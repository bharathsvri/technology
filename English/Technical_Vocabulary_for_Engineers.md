# Technical Vocabulary for Engineers (Meetings and Demos)

Using **precise** words reduces rework: distinguish **latency** vs **throughput**, **bug** vs **regression**, **rollback** vs **rollforward**.

---

## Performance

- **Latency**: time for **one** request.
- **Throughput**: **requests per second** sustained.
- **P99 / P95**: tail latency percentiles—**SLIs** often target these.

---

## Reliability

- **Incident**: unplanned degradation.
- **Outage**: user-visible **unavailability** (subset of incidents).
- **SLO / SLA / SLI**: objectives, contracts, indicators—**do not** conflate in exec updates.

---

## Delivery

- **MVP**: smallest releasable slice—**not** “rough draft forever.”
- **Canary / blue-green**: deployment **risk** reduction strategies.

---

## Related

- [Meetings and Status Updates](Meetings_and_Status_Updates.md)
- [Presentations and Public Speaking](Presentations_and_Public_Speaking.md)
