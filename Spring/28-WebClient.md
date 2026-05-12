# WebClient

**`WebClient`** is the reactive HTTP client used with WebFlux (and usable from MVC for non-blocking outbound calls). It returns **`Mono`** / **`Flux`** of responses.

---

## Creation

```java
WebClient client = WebClient.builder()
    .baseUrl("https://api.example.com")
    .defaultHeader(HttpHeaders.AUTHORIZATION, "Bearer " + token)
    .build();
```

Boot provides **`WebClient.Builder`** bean for injection.

---

## Retrieve pattern

```java
Mono<Order> order = client.get()
    .uri("/orders/{id}", id)
    .retrieve()
    .bodyToMono(Order.class);
```

---

## Error handling

```java
.retrieve()
.onStatus(HttpStatusCode::is4xxClientError, resp -> Mono.error(new NotFound()))
.bodyToMono(Order.class);
```

---

## Exchange pattern

`exchangeToMono` for full `ClientResponse` control (headers, status, selective body read).

---

## Timeouts

Configure **`HttpClient`** (Reactor Netty) connect/read/write timeouts—defaults can hang indefinitely.

---

## Next

- [RestTemplate and HTTP Clients](29-RestTemplate-and-HTTP-Clients.md)
