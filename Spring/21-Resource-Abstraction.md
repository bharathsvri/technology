# Resource Abstraction

Spring’s **`Resource`** abstraction reads **`classpath:`**, **`file:`**, **`http(s):`**, **`ServletContext`**, etc. uniformly—useful for templates, certs, batch files, and test fixtures.

---

## `ResourceLoader`

Inject **`ResourceLoader`**:

```java
Resource banner = resourceLoader.getResource("classpath:banner.txt");
try (InputStream in = banner.getInputStream()) {
    // read
}
```

---

## `ResourcePatternResolver`

Subclass of `ResourceLoader` supporting **`classpath*:`** patterns:

```java
Resource[] xmls = resolver.getResources("classpath*:META-INF/spring/*.xml");
```

---

## `Resource` methods

| Method | Purpose |
|--------|---------|
| **`exists()`** | Avoid missing file errors |
| **`isReadable()`** | Check permissions |
| **`getFile()`** | Only when backed by real file (not inside JAR for some resources) |
| **`getInputStream()`** | Universal read |

---

## `FileSystemResource` vs `ClassPathResource`

- **`ClassPathResource`** — packaged inside JAR; portable.
- **`FileSystemResource`** — externalized config on disk.

---

## Next

- [Environment, PropertySources, Profiles](22-Environment-PropertySources-and-Profiles.md)
