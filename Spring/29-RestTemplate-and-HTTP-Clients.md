# RestTemplate and HTTP Clients (Blocking)

**`RestTemplate`** is the classic **blocking** HTTP client in Spring. It is in **maintenance mode**; for new code Spring recommends **`RestClient`** (Spring Framework 6.1+) or **`WebClient`** for reactive stacks.

---

## Basic usage

```java
RestTemplate rt = new RestTemplate();
String body = rt.getForObject("https://example.com/api", String.class);

Order order = rt.postForObject(
    "https://example.com/orders",
    new HttpEntity<>(request, headers),
    Order.class);
```

---

## `exchange` / `ResponseEntity`

```java
ResponseEntity<Order> res = rt.exchange(
    url, HttpMethod.GET, null, Order.class);
HttpStatusCode status = res.getStatusCode();
HttpHeaders headers = res.getHeaders();
```

---

## Error handling

Implement **`ResponseErrorHandler`** on `RestTemplate` to map 4xx/5xx to exceptions consistently.

---

## Connection pooling

Default `SimpleClientHttpRequestFactory` does not pool like a production HTTP client. Use **Apache HttpClient** / **OkHttp** request factory for real workloads.

---

## Next

- [Spring Framework vs Spring Boot](30-Spring-Framework-vs-Spring-Boot.md)
