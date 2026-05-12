# Mock Service Worker (MSW) with React Testing Library

**MSW** intercepts **network** at the **worker** or **Node** level with **declarative handlers**—more realistic than mocking **`fetch`** ad hoc in each test.

---

## Why teams adopt it

- Same handlers for **local dev**, **Storybook**, and **tests**.
- Assert **request payloads** and return **errors** consistently.

---

## RTL integration sketch

- Start server in **`beforeAll`**, reset handlers **`afterEach`**, close **`afterAll`**.
- Render component → fire user event → **`await screen.findBy...`** for async UI.

---

## Pitfalls

- **Handlers drift** from real API contracts—pair with **OpenAPI** or **contract tests**.
- **SSR** tests need **Node** intercept setup, not **browser** worker.

---

## Related

- [Testing with React Testing Library](17-Testing-React-Testing-Library.md)
- [Data Fetching Patterns](12-Data-Fetching-Patterns.md)
