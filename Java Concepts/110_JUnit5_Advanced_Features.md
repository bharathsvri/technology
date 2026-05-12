# JUnit 5 Advanced Features

This document extends basic JUnit knowledge (see `59_Testing_Basics.md`) with features commonly used in real services.

## 1. Nested Tests (`@Nested`)
Model scenarios as a tree: outer class sets up shared fixtures; inner classes represent branches.

```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class OrderServiceTest {

    @Nested
    class WhenInventoryAvailable {
        @Test
        void placesOrder() { assertTrue(true); }
    }

    @Nested
    class WhenOutOfStock {
        @Test
        void rejectsOrder() { assertTrue(true); }
    }
}
```

## 2. Parameterized Tests
### `@ParameterizedTest` + sources
- `@ValueSource`, `@CsvSource`, `@MethodSource`, `@ArgumentsSource`.

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

class MathTest {
    @ParameterizedTest
    @CsvSource({"1,1,2", "2,3,5"})
    void add(int a, int b, int expected) {
        assertEquals(expected, a + b);
    }
}
```

## 3. Dynamic Tests (`@TestFactory`)
Generate tests at runtime when the cases come from data providers or parsers.

```java
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.TestFactory;
import java.util.stream.Stream;

import static org.junit.jupiter.api.DynamicTest.dynamicTest;

class DynamicExample {
    @TestFactory
    Stream<DynamicTest> generated() {
        return Stream.of("a", "b")
            .map(s -> dynamicTest("len " + s, () -> assertEquals(1, s.length())));
    }
}
```

## 4. Lifecycle and Extension Model
- **`@BeforeEach` / `@AfterEach`**: per test method.
- **`@BeforeAll` / `@AfterAll`**: per class; for expensive resources, pair with `static` methods or **`@TestInstance(TestInstance.Lifecycle.PER_CLASS)`** to avoid static.

### Extensions
Implement `BeforeEachCallback`, `ParameterResolver`, etc., for reusable wiring (testcontainers, mocks, clock injection).

## 5. Assumptions
Skip tests when environment prerequisites fail (OS, feature flag, external dependency).

```java
import static org.junit.jupiter.api.Assumptions.assumeTrue;

@Test
void onlyOnLinux() {
    assumeTrue(System.getProperty("os.name").toLowerCase().contains("linux"));
}
```

## 6. Parallel Execution (JUnit Platform)
Configured via `junit-platform.properties`:
- `junit.jupiter.execution.parallel.enabled=true`
- `junit.jupiter.execution.parallel.mode.default=concurrent`

Ensure tests are **isolated** (no shared mutable static state).

## 7. Tags and Build Filtering
Annotate with `@Tag("integration")` and filter in Gradle/Maven surefire for fast default builds.

## 8. Architecture / Structural Tests (with ArchUnit)
While ArchUnit is a separate library, it pairs naturally with JUnit 5:

```java
import com.tngtech.archunit.junit.AnalyzeClasses;
import com.tngtech.archunit.junit.ArchTest;
import com.tngtech.archunit.lang.ArchRule;
import static com.tngtech.archunit.library.dependencies.SlicesRuleDefinition.slices;

@AnalyzeClasses(packages = "com.example")
class ArchitectureTest {
    @ArchTest
    static final ArchRule noCycles =
        slices().matching("com.example.(*)..").should().beFreeOfCycles();
}
```

## Best Practices
- Prefer **small unit tests** + selective **integration tests**; use tags to split CI stages.
- Use **`assertAll`** for grouped assertions that should all run even after a failure.
- Keep parameterized cases **readable**; extract method sources for large tables.
