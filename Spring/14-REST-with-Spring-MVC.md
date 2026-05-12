# Building REST APIs with Spring MVC

REST APIs return **resources** as JSON/XML (usually JSON) over HTTP with meaningful **status codes** and **headers**. Spring MVC uses **`HttpMessageConverter`** (Jackson by default) to serialize/deserialize bodies.

---

## Status codes with `ResponseEntity`

```java
@PostMapping("/payments")
public ResponseEntity<PaymentDto> pay(@RequestBody PaymentRequest req) {
    PaymentDto dto = service.charge(req);
    return ResponseEntity
        .status(HttpStatus.CREATED)
        .location(URI.create("/payments/" + dto.id()))
        .body(dto);
}
```

| Code | Typical meaning |
|------|-----------------|
| **200 OK** | Successful GET/PUT/PATCH |
| **201 Created** | POST created resource |
| **204 No Content** | DELETE success, no body |
| **400 Bad Request** | Validation / malformed input |
| **404 Not Found** | Missing resource |
| **409 Conflict** | Version / uniqueness conflict |

---

## Problem Details (RFC 9457)

Spring Boot 3+ supports standardized error bodies via **`ProblemDetail`** — see `Spring Boot Concepts` doc on RFC 9457 in this repo.

---

## Content negotiation

`produces = MediaType.APPLICATION_JSON_VALUE` on mapping; clients send `Accept: application/json`.

---

## CORS

**`@CrossOrigin`** on controller/method, or global `WebMvcConfigurer.addCorsMappings` for allowed origins, methods, headers.

---

## Validation

`@Valid` on `@RequestBody` triggers **`MethodArgumentNotValidException`** — handle in `@ControllerAdvice` to return field errors consistently.

---

## Versioning strategies

- URL prefix `/v1/orders`
- Header `Accept: application/vnd.company.v1+json`
- Query parameter (least REST-purist but simple)

Pick one; document in OpenAPI.

---

## Next

- [Exception Handling in Spring MVC](15-Exception-Handling-in-Spring-MVC.md)
