# Web Vitals and Measuring React Performance

**Core Web Vitals** (**LCP**, **INP**, **CLS**) quantify user-perceived performance. React apps optimize differently than static sites because **hydration** and **re-renders** affect **INP**.

---

## Definitions (interview crisp)

- **LCP**: largest contentful paint—hero image/text.
- **INP**: interaction to next paint (replaced FID as responsiveness metric).
- **CLS**: layout shift cumulative score.

---

## React-specific levers

- **Defer non-critical JS** (route-level code splitting, RSC where applicable).
- **Avoid main-thread long tasks** after clicks—**transitions**, **memoization**, **list virtualization**.
- **Measure in field** with **`web-vitals`** library reporting to analytics.

---

## Tooling

React **Profiler** (dev) for commit times; **Lighthouse** in CI for regressions; **Real User Monitoring** for distributions.

---

## Related

- [Performance Optimization](16-Performance-Optimization.md)
- [React 18 Concurrent Features](19-React-18-Concurrent-Features.md)
