# Text Blocks — Indentation and `stripIndent`

**Text blocks** (`""" ... """`) preserve **relative** indentation; **`stripIndent()`** normalizes incidental left margin so SQL/HTML embedded in Java reads cleanly.

---

## Rules of thumb

- Align closing **`"""`** with the **least-indented** line of content, or use **`stripIndent`** / IDE formatting.
- **Trailing whitespace** on each line is usually stripped; explicit spaces matter for **markdown** / **SQL**.

---

## `formatted` vs concatenation

**`"text".formatted(args)`** pairs nicely with blocks for **parameterized** snippets.

---

## Related

- [Text Blocks](47_Text_Blocks.md)
- [String Manipulation](19_String_Manipulation.md)
