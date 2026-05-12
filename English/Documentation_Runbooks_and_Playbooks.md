# Runbooks, Playbooks, and Operational Documentation

**Runbooks** are **step-by-step** procedures for **known** scenarios (restart service, fail over replica). **Playbooks** often include **decision trees** for incidents—both reduce **tribal knowledge**.

---

## What good runbooks contain

- **Scope**: when to use vs escalate.
- **Prerequisites**: access, tools, dashboards links.
- **Commands** with **copy-paste** safety notes (read-only vs mutating).
- **Verification**: how to know success (**metrics**, **health checks**).
- **Rollback**: how to undo.

---

## Keep them honest

- **Dry-run** after major infra changes.
- **Version** in git next to code—stale runbooks are worse than none.

---

## Related

- [On Call and Incident Communication](On_Call_and_Incident_Communication.md)
- [Technical Writing and Documentation](Technical_Writing_and_Documentation.md)
