# `watch` vs `watchEffect` — Decision Guide

Both APIs react to dependency changes, but **`watch`** is **explicit** about sources while **`watchEffect`** **auto-tracks** reads on each run.

---

## Choose **`watch`** when

- You need **old vs new** values.
- You want **only specific** fields to trigger work (avoid accidental deps).
- The team prefers **grep-friendly** dependency lists in code review.

---

## Choose **`watchEffect`** when

- Dependencies are **many** or **dynamic** and listing them is noisy.
- You want an **immediate** run that sets up **logging** / **sync** tied to whatever reads this cycle.

---

## Performance and clarity

- **`watchEffect`** can **over-subscribe** if the callback reads broad state—causes extra runs.
- **`watch` + `deep: true`** on huge objects is expensive—prefer **narrow** sources or **computed** slices.

---

## Related

- [Watchers](05-Watchers.md)
- [Reactivity ref reactive computed](04-Reactivity-ref-reactive-computed.md)
