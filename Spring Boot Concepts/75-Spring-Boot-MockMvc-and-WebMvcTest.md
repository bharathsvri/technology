# Spring Boot: MockMvc and `@WebMvcTest`

**MockMvc** tests the **web slice** of Spring MVC without starting a real HTTP port: it exercises **filters**, **interceptors**, **`@ControllerAdvice`**, serialization, and status codes through a servlet API façade.

## `@WebMvcTest`
Loads **only** MVC-related beans (controllers, JSON converters, security filter chain when imported)—fast compared to `@SpringBootTest`.

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(OrderController.class)
class OrderControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    void getOrder() throws Exception {
        mockMvc.perform(get("/api/orders/1"))
            .andExpect(status().isOk());
    }
}
```

## Security in Slice Tests
Import **`SecurityMockMvcRequestPostProcessors`** or a minimal security test configuration when controllers are secured.

## `RestTestClient` (Spring 6.2+ / Boot alignment)
For a more fluent HTTP-style API over the same infrastructure, some teams prefer **`RestTestClient`**—check your Spring Boot version’s testing appendix for availability and examples.

## When to Use Full Stack Tests
Use **`@SpringBootTest(webEnvironment = RANDOM_PORT)`** + `TestRestTemplate`/`WebTestClient` when integration across **JPA**, **security**, and **HTTP** is the goal (`55-Spring-Boot-Integration-Testing.md`).

## Related Topics
- `13-Spring-Boot-Testing.md`
- `55-Spring-Boot-Integration-Testing.md`
- `56-Spring-Boot-Annotations-Complete-Guide.md`
