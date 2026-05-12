# Writing RFCs and ADRs

**RFC (Request for Comments)** and **ADR (Architecture Decision Record)** turn **implicit debates** into **durable artifacts**—critical for staff-level communication and onboarding.

---

## RFC — when to use

- **Cross-team** API or platform change with alternatives and rollout risk.
- Sections: **context**, **proposal**, **alternatives rejected**, **migration**, **open questions**, **reviewers**.

**Goal**: async feedback before **committing** org-wide direction.

---

## ADR — when to use

- **Record a decision** after choice is made: **status**, **context**, **decision**, **consequences**.
- Keep **short** (1–2 pages); link to RFC or design doc for depth.

**Number ADRs** (`0001-use-postgres-for-orders.md`) for easy reference in PRs.

---

## Habits that work

- Time-box RFC review (**one week**) with **explicit owners** for each open question.
- Update ADR **status** to **superseded** when reversing course—history matters.

---

## Related

- [Technical Writing and Documentation](Technical_Writing_and_Documentation.md)
- [Code Review and Pull Request Etiquette](Code_Review_and_Pull_Request_Etiquette.md)
