# `equals` and `hashCode` Contract

Correct **`equals`** and **`hashCode`** implementations are essential for **`HashMap`**, **`HashSet`**, distributed caches, and persistence layers.

## `equals` Rules (Reflexive, Symmetric, Transitive, Consistent, Null-safe)
For non-null references **x, y, z**:
- **Reflexive**: `x.equals(x)` is true.
- **Symmetric**: `x.equals(y)` iff `y.equals(x)`.
- **Transitive**: if `x.equals(y)` and `y.equals(z)`, then `x.equals(z)`.
- **Consistent**: repeated calls return the same result if fields used are unchanged.
- For any non-null **x**, `x.equals(null)` is **false**.

## `hashCode` Contract
- If two objects are **equal** via `equals`, they **must** have the **same** `hashCode`.
- The converse is **not** required: unequal objects may collide on `hashCode` (but good hashes reduce collisions).

## Practical Recipe (Fields)
1. Start with a **non-zero** constant (e.g., `31` tradition in `Objects.hash` style).
2. Combine **significant fields** the same way in both methods.
3. Prefer **`Objects.equals`** / **`Objects.hash`** for brevity; for hot types, hand-roll to avoid array allocation.
4. **Do not** include fields excluded from equality (e.g., cache or ORM version column unless part of identity).

## Subtyping Pitfall
If `equals` is overridden in a subclass with extra state, **symmetry** breaks unless you use a pattern like **composition** instead of inheritance for identity, or carefully document **canonical forms** (rare). Often prefer **`final`** classes for value types.

## Lombok
`@EqualsAndHashCode` generates consistent methods—still choose **`onlyExplicitlyIncluded = true`** when you want stable identity across ORM proxies.

## Related Topics
- `10_Classes_and_Objects.md`
- `98_Lombok.md`
- `106_Collections_Architecture_and_Internal_Working.md`
