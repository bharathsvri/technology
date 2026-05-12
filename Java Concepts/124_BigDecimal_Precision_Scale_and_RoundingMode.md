# BigDecimal: Precision, Scale, and Rounding

Use **`BigDecimal`** for **money**, **tax**, **rates**, and any decimal arithmetic where **`double` is unacceptable** (binary floating-point cannot represent many decimals exactly).

## Core Terms
- **Precision**: total number of significant digits.
- **Scale**: digits to the **right** of the decimal point (can be negative for values like `1E+3`).

## Construction Traps
Prefer **`BigDecimal.valueOf(0.1)` is still wrong for literals**—`valueOf(double)` uses the `double`’s binary approximation.

**Safe patterns**:
- `new BigDecimal("19.99")` from **strings**.
- `BigDecimal.valueOf(1999L, 2)` from **unscaled** long + scale.

## Arithmetic and Rounding
`divide` requires a **`RoundingMode`** when the exact quotient needs infinite precision:

```java
BigDecimal a = new BigDecimal("10");
BigDecimal b = new BigDecimal("3");
BigDecimal q = a.divide(b, 4, RoundingMode.HALF_UP);
```

Common modes: **`HALF_UP`** (typical commercial), **`HALF_EVEN`** (banker’s rounding), **`DOWN`** for conservative bounds.

## Money Practices
- Store **minor units** (cents) as integers when policy allows—fastest and least ambiguous.
- If using `BigDecimal`, standardize **scale** (e.g., 2 for USD) at persistence boundaries.
- Never mix **`double`** math with `BigDecimal` in hot financial paths.

## Related Topics
- `02_Data_Types.md`
- `83_JTA.md` (transactional transfers often use `BigDecimal`)
