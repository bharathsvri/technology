# `java.net.http.HttpClient` vs `HttpURLConnection`

**`HttpClient`** (Java 11+) supports **HTTP/2**, **async** (`sendAsync`), **timeouts**, and a **fluent** API—preferred over legacy **`HttpURLConnection`** for new code.

---

## Synchronous send

```java
var client = HttpClient.newBuilder().connectTimeout(Duration.ofSeconds(5)).build();
var req = HttpRequest.newBuilder(URI.create(url)).GET().build();
HttpResponse<String> res = client.send(req, HttpResponse.BodyHandlers.ofString());
```

---

## When `HttpURLConnection` still appears

Legacy stacks, very **old** Android targets, or **corporate** proxies requiring **esoteric** workarounds—**migrate** when feasible.

---

## Spring alignment

**`RestClient`** / **`WebClient`** wrap similar concerns with **Spring** integration—know the **JDK** layer underneath.

---

## Related

- [Networking](37_Networking.md)
- [Apache HttpClient](101_Apache_HttpClient.md)
