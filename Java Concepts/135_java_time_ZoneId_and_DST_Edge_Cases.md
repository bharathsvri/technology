# `java.time`: `ZoneId`, Offsets, and DST Edge Cases

**`ZonedDateTime`** binds an **instant** to a **region’s rules** (DST shifts). Interviews probe **parsing**, **storage**, and **arithmetic** pitfalls.

---

## Store **UTC**, display local

Persist **`Instant`** or **epoch millis** + canonical zone id; render with user’s **`ZoneId`**.

---

## `Gap` vs `Overlap` (DST)

- **Spring forward**: **nonexistent** local times—`ZonedDateTime` resolves via **`ResolverStyle`** / offset rules.
- **Fall back**: **ambiguous** local clock times—**disambiguate** with policy.

---

## `ZoneOffset` vs `ZoneId`

**`ZoneOffset.ofHours(5)`** ignores **DST**—fine for fixed-offset domains (some finance), wrong for **civil** time in most countries.

---

## Related

- [Date and Time API](26_Date_Time_API.md)
- [Internationalization](49_Internationalization.md)
