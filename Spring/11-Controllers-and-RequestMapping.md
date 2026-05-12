# Controllers and `@RequestMapping`

Controllers are **`@Controller`** or **`@RestController`** beans whose methods map to HTTP endpoints via **`@RequestMapping`** and composed annotations.

---

## `@RestController`

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {

    private final OrderService orders;

    public OrderController(OrderService orders) {
        this.orders = orders;
    }

    @GetMapping("/{id}")
    public Order get(@PathVariable long id) {
        return orders.find(id);
    }

    @PostMapping(consumes = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<Order> create(@RequestBody CreateOrderRequest body) {
        Order saved = orders.create(body);
        return ResponseEntity.created(URI.create("/api/orders/" + saved.getId())).body(saved);
    }
}
```

`@RestController` = `@Controller` + **`@ResponseBody`** on every handler method (return value serialized to response body).

---

## HTTP method shortcuts

| Annotation | Equivalent to |
|------------|----------------|
| `@GetMapping("/x")` | `@RequestMapping(path="/x", method=GET)` |
| `@PostMapping` | `POST` |
| `@PutMapping` | `PUT` |
| `@PatchMapping` | `PATCH` |
| `@DeleteMapping` | `DELETE` |

---

## `consumes` and `produces`

Restrict mapping by **`Content-Type`** and **`Accept`** headers—useful for versioning or multiple representations.

```java
@GetMapping(value = "/{id}", produces = MediaType.APPLICATION_JSON_VALUE)
```

---

## Class-level vs method-level mapping

Paths **combine**: class `/api` + method `/orders` → `/api/orders` (with slash normalization rules—verify in docs for your version).

---

## Produces and content negotiation

If multiple methods could match, **produces/consumes** narrows selection; otherwise ambiguous mapping errors occur at startup.

---

## Next

- [Request Parameters, Path Variable, and Body](12-Request-Parameters-Path-Variable-and-Body.md)
