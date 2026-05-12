# On-Call and Incident Communication

**Incidents** are stress tests of communication: customers, leadership, and teammates infer reliability from **clarity and cadence**, not heroics.

---

## Roles (typical)

- **Incident Commander**: owns timeline, decisions, delegation.
- **Communications**: status updates outward.
- **Scribe**: timeline, hypotheses, actions (feeds postmortem).

Titles vary; knowing **single-decider** and **single voice** to customers reduces confusion.

---

## Internal updates

- **First message**: impact, **severity**, **unknowns**, **next update time**.
- **Steady cadence** every N minutes even if “no change” — silence reads as **lost control**.
- Log **UTC timestamps**, **versions deployed**, **feature flags flipped**.

---

## Customer / exec comms

- **Facts** over speculation; separate **user impact** from **internal cause**.
- **ETA ranges** or **“we will update by X”** instead of false precision.

---

## After resolution

- **Postmortem** without blame; **action items** with owners and dates.
- Share **lessons** in engineering-wide forum when broadly applicable.

---

## Related

- [Remote and Async Communication](Remote_and_Async_Communication.md)
- [Client Communication Guide](Client_Communication_Guide.md)
