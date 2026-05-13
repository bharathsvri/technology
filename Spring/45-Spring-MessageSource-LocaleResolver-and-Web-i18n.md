# Spring: `MessageSource`, `LocaleResolver`, and web i18n

## Why it matters
Even mostly-JSON APIs return **validation messages**, **problem details**, and sometimes **HTML**. Spring MVC centralizes **locale choice** and **message catalogs** so controllers stay thin.

## `MessageSource`
- **`ApplicationContext`** extends `MessageSource`; most apps inject **`MessageSource`** or use **`MessageSourceAccessor`**.
- Messages live in **`classpath:messages*.properties`** (Basename configurable via `spring.messages.basename`).
- Validation annotations can reference keys: `message = "{order.id.positive}"` (`17-Validation-with-Spring.md`).

### Hot reload vs cache
In production, message bundles are effectively static; in dev you may want **reloadable** resource bundles—balance against performance.

## Choosing a locale: `LocaleResolver`
Spring MVC consults a **`LocaleResolver`** bean to determine **`Locale`** per request. Common strategies:

| Resolver style | When it fits |
|----------------|--------------|
| **`AcceptHeaderLocaleResolver`** | APIs where the client sends **`Accept-Language`** |
| **`CookieLocaleResolver`** | Browser apps with a language toggle stored in a cookie |
| **`SessionLocaleResolver`** | Server sessions already in use; less ideal for stateless APIs |

You can still override per request with **`LocaleChangeInterceptor`** reading a query parameter such as `?lang=fr`.

## REST vs MVC views
- **REST**: return **RFC 9457** bodies (`Spring Boot Concepts/62-Spring-Boot-Problem-Details-RFC9457.md`) with machine codes; localize **human** `detail` fields carefully—clients may ignore them.
- **MVC + Thymeleaf / FreeMarker**: localize **templates** and shared fragments.

## Testing
Use **`@AutoConfigureMockMvc`** with **`Accept-Language`** headers or cookie builders to assert localized output. Keep golden strings in **tests** stable (pick a fixed locale in the test).

## Related docs
- `22-Environment-PropertySources-and-Profiles.md` — profile-specific basenames
- `15-Exception-Handling-in-Spring-MVC.md` — localized error models
- `23-Spring-Boot-Internationalization.md` in **Spring Boot Concepts** — Boot defaults for message bundles
