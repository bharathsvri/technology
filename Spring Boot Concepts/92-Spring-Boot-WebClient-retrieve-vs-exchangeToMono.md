# `WebClient` `retrieve()` vs `exchangeToMono()`

**`retrieve()`** is the **recommended** default: body decoded with **status checks** via **`onStatus`**. **`exchangeToMono()`** exposes **`ClientResponse`** for **advanced** flows but makes it easier to **forget** to **consume** the body (connection leaks).

---

## Rule of thumb

- **Normal REST** calls → **`retrieve().bodyToMono(...)`**.
- **Conditional** handling of **content types** or **headers** before committing to a decoder → **`exchangeToMono`** with discipline.

---

## Memory / streaming

Large downloads use **`bodyToFlux(DataBuffer.class)`** with **backpressure** awareness.

---

## Related

- [WebClient](../Spring/28-WebClient.md)
- [Spring Boot WebFlux](37-Spring-Boot-WebFlux.md)
