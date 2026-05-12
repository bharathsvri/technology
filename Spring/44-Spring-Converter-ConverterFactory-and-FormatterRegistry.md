# `Converter`, `ConverterFactory`, and `FormatterRegistry`

Spring’s **conversion service** bridges **strings** in requests/responses to **domain types** (enums, money, ISO dates). **`Converter<S,T>`** is one-to-one; **`ConverterFactory`** creates converters for **families** of source types.

---

## Web layer

**`FormatterRegistry`** registers **formatters** for **locale-sensitive** parsing in MVC (often via **`@DateTimeFormat`** under the hood).

---

## Custom converter bean

Expose **`Converter<String, MyId>`** as **`@Bean`**—picked up by **`FormattingConversionService`**.

---

## Pitfalls

- **Too greedy** converters can break **ambiguous** strings—keep **narrow** types.
- **Order** matters when multiple converters match—**test** edge cases.

---

## Related

- [Validation with Spring](17-Validation-with-Spring.md)
- [HTTP Message Converters](16-HTTP-Message-Converters.md)
