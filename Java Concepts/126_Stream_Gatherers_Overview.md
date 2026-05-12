# Stream Gatherers (Java 24+)

**Gatherers** extend the `Stream` API with **custom intermediate reductions** beyond what `map`, `filter`, `collect`, and built-in collectors express cleanly—useful for **windowed**, **stateful**, or **grouped** passes in a single stream pipeline.

> **Status**: Finalized in **JDK 24** as [JEP 485: Stream Gatherers](https://openjdk.org/jeps/485). Preview iterations appeared in JDK 22–23.

## Mental Model
- A **gatherer** defines **initializer**, **integrator**, **combiner**, and **finisher** logic (names vary by API surface) to consume stream elements and emit **downstream** elements or a final result.
- Contrast with **`Collector`**: collectors usually terminate into a mutable container; gatherers are positioned as a **middle** stage with more control over **short-circuiting** and **parallel** splitting.

## When to Consider Gatherers
- Custom **sliding** or **session** windows not covered by `Stream` helpers.
- Stateful transforms that are awkward with `peek` side effects.

## When to Skip Them
- If a **plain loop** or **reactive** pipeline (`62_Reactive_Programming.md`) is clearer for the team—advanced stream stages can hurt readability.

## Related Topics
- `23_Streams_API.md`, `38_Advanced_Streams.md`
- `104_Java_Versions_New_Features.md`
