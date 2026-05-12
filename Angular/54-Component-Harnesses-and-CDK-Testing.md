# Component Harnesses and CDK Testing

**Component Harnesses** (@angular/cdk/testing) provide a **stable, role-like API** for tests instead of querying raw DOM with brittle CSS selectors. Especially valuable for **Angular Material** and **custom** design systems.

---

## Why interviewers like harnesses

- Tests survive **template refactors** that keep the public “contract” (button label, input role) the same.
- Parallel-friendly **async** interaction model (`await harness.click()`).

---

## Sketch

```ts
// Conceptual: loader finds harness under root
const loader = TestbedHarnessEnvironment.loader(fixture);
const buttonHarness = await loader.getHarness(MatButtonHarness.with({ text: 'Save' }));
await buttonHarness.click();
```

Harness authors implement **`ComponentHarness`** with **`locatorFor`** / **`locatorForOptional`**.

---

## When to use

| Use harnesses | Raw `@vue/test-utils` style queries OK |
|---------------|----------------------------------------|
| Material / CDK components | Tiny leaf components |
| Cross-team design system | Prototype throwaway |

---

## Related

- [Angular CDK](32-Angular-CDK.md)
- [Testing with Jasmine and Karma](24-Testing-Jasmine-Karma.md)
- [Testing with Jest or Vitest](42-Testing-with-Jest-or-Vitest.md)
