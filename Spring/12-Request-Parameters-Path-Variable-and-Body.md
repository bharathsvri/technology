# Request Parameters, Path Variable, and Body

Spring MVC **argument resolvers** turn raw HTTP data into Java parameters. Understanding each annotation avoids subtle binding bugs.

---

## `@RequestParam`

Binds **query string** or **form** parameters.

```java
@GetMapping("/search")
public List<Order> search(
    @RequestParam String q,
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(required = false) String sort) { ... }
```

- **`required=false`** — optional.
- **`defaultValue`** — used when parameter missing (also implies not required).
- **`name` / `value`** — alias for query key.

Arrays: `@RequestParam List<String> tag` from repeated `?tag=a&tag=b`.

---

## `@PathVariable`

Binds URI template variables:

```java
@GetMapping("/users/{userId}/orders/{orderId}")
public Order get(@PathVariable long userId, @PathVariable long orderId) { ... }
```

Optional: `@PathVariable(required = false)` only if the path segment is optional in the pattern.

---

## `@RequestBody`

Deserializes the **HTTP body** (usually JSON) via `HttpMessageConverter` (Jackson) into a POJO or `Map`.

```java
@PostMapping
public Order create(@Valid @RequestBody CreateOrderRequest body) { ... }
```

Requires correct **`Content-Type`** (e.g. `application/json`). Use **`@Valid`** with Bean Validation.

---

## `@ModelAttribute`

Binds request parameters to an **object** for traditional form posts; also adds the object to the model for views.

---

## `@RequestHeader` and `@CookieValue`

```java
public void track(@RequestHeader("User-Agent") String ua, @CookieValue("sid") String sessionId) { ... }
```

---

## `Optional<T>` parameters

Spring can inject **`Optional`** for query/path parameters in many versions—still document API clearly for OpenAPI consumers.

---

## Pitfalls

- **GET + `@RequestBody`** — technically allowed by HTTP but discouraged; caches/proxies may strip bodies.
- **Missing `@RequestBody`** on complex JSON DTO — Spring may try to bind query/form fields instead and fail mysteriously.

---

## Next

- [View Resolution](13-View-Resolution.md)
