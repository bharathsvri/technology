# Aspect-Oriented Programming (AOP) in Spring

**AOP** modularizes **cross-cutting concerns** (logging, transactions, security, metrics) that would otherwise be **duplicated** across many classes. Spring AOP is **proxy-based**: at runtime your bean is wrapped in a **JDK dynamic proxy** (interfaces) or **CGLIB subclass** (concrete classes).

---

## Core vocabulary

| Term | Meaning |
|------|---------|
| **Aspect** | Module encapsulating advice + pointcuts |
| **Join point** | A point in execution (e.g. method call) |
| **Pointcut** | Predicate selecting join points (`execution(...)`) |
| **Advice** | Code to run at join points (`@Before`, `@Around`, …) |
| **Weaving** | Applying aspects — Spring does this at **runtime** via proxies (not compile-time AspectJ for default Spring AOP) |

---

## Proxy types

- **JDK dynamic proxy**: bean implements an **interface** — only **interface methods** are intercepted.
- **CGLIB**: subclass of **concrete class** — `final` classes/methods **cannot** be proxied.

If `@Transactional` “does not work”, common causes are **self-invocation** (calling `this.method()` from same class — bypasses proxy) or **non-public** methods on CGLIB.

---

## Example: `@Around` timing

```java
@Aspect
@Component
public class ServiceTimingAspect {

    private static final Logger log = LoggerFactory.getLogger(ServiceTimingAspect.class);

    @Around("execution(* com.example..service..*(..))")
    public Object time(ProceedingJoinPoint pjp) throws Throwable {
        long t0 = System.nanoTime();
        try {
            return pjp.proceed();
        } finally {
            long ms = (System.nanoTime() - t0) / 1_000_000;
            log.info("{} took {} ms", pjp.getSignature().toShortString(), ms);
        }
    }
}
```

Enable aspect processing with **`@EnableAspectJAutoProxy`** on a `@Configuration` class (Boot may auto-enable when `spring-boot-starter-aop` is present).

---

## Advice types

| Annotation | Runs |
|------------|------|
| `@Before` | Before join point |
| `@AfterReturning` | After normal return |
| `@AfterThrowing` | After exception |
| `@After` (finally) | Always after join point |
| `@Around` | Wraps join point — **must** call `proceed()` |

`@Around` is the most powerful: you can skip `proceed()`, catch exceptions, replace return values.

---

## Pointcut expressions (examples)

```java
@Pointcut("execution(public * com.example.billing..*(..))")
public void billingLayer() {}

@Pointcut("@annotation(com.example.audit.Audited)")
public void auditedMethods() {}
```

Combine: `@Pointcut("billingLayer() && auditedMethods()")`

---

## Limitations of Spring AOP

- Only **Spring-managed beans** are proxied — `new MyService()` is **not** advised.
- **Self-invocation** bypasses proxy — extract to another bean or use AspectJ weaving for true `this` interception.
- **private/final** methods on concrete classes may not be advised with CGLIB depending on visibility rules.

---

## Next

- [Spring MVC Architecture](09-Spring-MVC-Architecture.md)
