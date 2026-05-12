# Immutable Collections and Unmodifiable Views

Java offers **true immutable** collections (Java 9+ **`List.of`**, **`Set.of`**, **`Map.of`**) and **unmodifiable views** over **mutable** backing collections—interviews test whether you know the **difference**.

---

## Factory methods (`List.of`, `Set.of`, `Map.of`)

```java
List<String> xs = List.of("a", "b"); // immutable, no nulls
```

- **Fixed size**, **no null** elements (NPE on attempt).
- **Iteration order** defined for `List.of`; `Set.of` order unspecified.

---

## `Collections.unmodifiableList`

Wraps a **mutable** list; **wrapper** rejects `add`, but **caller with original reference** can still mutate—**not** a deep freeze.

```java
var inner = new ArrayList<String>();
inner.add("x");
var view = Collections.unmodifiableList(inner);
inner.add("y"); // mutates "immutable" view from observer POV
```

---

## `copyOf` (Java 10+)

```java
List<String> snap = List.copyOf(maybeMutable);
```

Creates **unmodifiable copy** if the source is not already known immutable—safe snapshot for defensive APIs.

---

## Interview angle

- Expose **`List.copyOf`** or **immutable return types** from service boundaries.
- **Records** + immutable collections = **value-oriented** DTOs.

---

## Related

- [Collections Framework](17_Collections_Framework.md)
- [Sequenced Collections](120_Sequenced_Collections_Java_21.md)
