# Spring Boot: Spring REST Docs (Test-Driven API Documentation)

## What It Is
**Spring REST Docs** generates documentation snippets (request/response examples, headers, fields) from **real tests** that exercise your controllers or REST clients. Output is usually combined with **Asciidoctor** or other templates to publish HTML or PDF.

It complements **OpenAPI** (`27-Spring-Boot-Swagger-OpenAPI.md`): OpenAPI describes the **contract** for tools and clients; REST Docs proves **what the server actually does** at a point in time, including edge cases you choose to demonstrate.

## Why Teams Use It
- Examples stay **aligned with behavior** because broken tests break the doc build.
- You can document **payload nuances** (optional fields, error shapes) without exposing internal types you do not want in a public schema.
- Works well for **hypermedia** or highly customized responses where generated OpenAPI is noisy.

## Typical Setup (High Level)
1. Add **`spring-restdocs-mockmvc`** (MVC) or **`spring-restdocs-webtestclient`** (WebFlux) test dependencies.
2. In tests, use **`MockMvc`** or **`WebTestClient`** with **`document()`** snippets (path parameters, request fields, response fields, links).
3. Run the **Asciidoctor** (or similar) task to assemble `.adoc` sources plus generated snippets into published docs.

## Practices That Pay Off
- Treat snippet directories as **build artifacts**; regenerate on CI.
- Version docs with the **API release**; tag Git alongside published doc bundles.
- For public APIs, many teams ship **both** OpenAPI (discovery/codegen) and REST Docs (human narrative).

## Related Docs
- `75-Spring-Boot-MockMvc-and-WebMvcTest.md` — MVC testing foundation.
- `55-Spring-Boot-Integration-Testing.md` — broader integration test strategy.
- `62-Spring-Boot-Problem-Details-RFC9457.md` — consistent error payloads worth documenting.
