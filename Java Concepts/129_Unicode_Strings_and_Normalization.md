# Unicode, Strings, and Normalization in Java

**`java.lang.String`** is UTF-16; **Unicode** code points may span **one or two `char`** units (**surrogate pairs**). Interviews touch **normalization**, **locale-sensitive** operations, and **correct length/grapheme** handling.

---

## Code points vs `char`

```java
int cp = str.codePointAt(i);
boolean letter = Character.isLetter(cp);
```

Use **`codePoints()`** stream for full-string logic that cannot assume BMP-only.

---

## Normalization (`java.text.Normalizer`)

**NFC vs NFD** (composed vs decomposed) matters when comparing **user input**, **file names**, or **macOS** paths.

```java
String nfc = Normalizer.normalize(input, Normalizer.Form.NFC);
```

---

## `String.strip()` vs `trim()`

- **`strip()`** (Java 11): Unicode **whitespace** aware.
- **`trim()`**: legacy **≤ U+0020** space semantics.

---

## Collation

Sorting **German** vs **Turkish** **I/İ** differs—use **`Collator.getInstance(locale)`** for user-visible ordering, not **`String.compareTo`** for i18n lists.

---

## Related

- [String Manipulation](19_String_Manipulation.md)
- [Internationalization](49_Internationalization.md)
