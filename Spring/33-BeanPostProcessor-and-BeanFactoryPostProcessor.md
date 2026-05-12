# `BeanPostProcessor` and `BeanFactoryPostProcessor`

These extension points let frameworks and advanced apps **modify bean definitions** or **wrap bean instances** during context startup—used by **AOP proxies**, **@ConfigurationProperties` binding**, and **testing** utilities.

---

## `BeanFactoryPostProcessor`

- Operates on the **`ConfigurableListableBeanFactory`** **before** most beans are instantiated.
- Typical use: adjust **property placeholders**, register **BeanDefinitionRegistryPostProcessor**, tweak **bean definitions**.

**Order**: implement **`Ordered`** / **`PriorityOrdered`**—ordering bugs cause subtle failures.

---

## `BeanPostProcessor`

- Hooks **after** bean construction: **`postProcessBeforeInitialization`** / **`postProcessAfterInitialization`**.
- Classic use: **wrap** beans with proxies (validation, metrics), **`@Autowired`** injection into fields after construction.

**Pitfall**: BPPs must be **registered early**; beans created before BPP registration won’t be wrapped.

---

## Interview caution

Prefer **annotation-driven** Spring Boot features over custom BPPs unless you are building **framework code**.

---

## Related

- [Bean Lifecycle and Scopes](05-Bean-Lifecycle-and-Scopes.md)
- [Aspect-Oriented Programming](08-Aspect-Oriented-Programming.md)
