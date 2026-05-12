# Exception Handling in Spring MVC

Centralized exception handling keeps **HTTP status**, **body shape**, and **logging** consistent across controllers.

---

## `@ControllerAdvice` / `@RestControllerAdvice`

```java
@RestControllerAdvice
public class ApiExceptionHandler {

    @ExceptionHandler(NotFoundException.class)
    public ProblemDetail notFound(NotFoundException ex, HttpServletRequest req) {
        ProblemDetail pd = ProblemDetail.forStatusAndDetail(404, ex.getMessage());
        pd.setInstance(URI.create(req.getRequestURI()));
        return pd;
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ProblemDetail validation(MethodArgumentNotValidException ex) {
        // map field errors to extension properties
        return ProblemDetail.forStatus(400);
    }
}
```

`@RestControllerAdvice` = `@ControllerAdvice` + **`@ResponseBody`** semantics on handler methods.

---

## `@ResponseStatus` on exception types

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class NotFoundException extends RuntimeException { }
```

Quick but less flexible than `ProblemDetail` bodies.

---

## `HandlerExceptionResolver` SPI

Lower-level than `@ControllerAdvice`; rarely needed unless integrating non-standard frameworks.

---

## Logging

- Log **stack traces** at **ERROR** for unexpected exceptions.
- For **client errors** (4xx), often **INFO** with sanitized message—avoid logging PII or full card numbers.

---

## Do not leak internals in production

Disable stack traces in JSON error bodies; return **generic** message to clients and track correlation id internally.

---

## Next

- [HTTP Message Converters](16-HTTP-Message-Converters.md)
