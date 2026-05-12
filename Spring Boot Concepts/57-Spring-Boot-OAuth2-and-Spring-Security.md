# Spring Boot: OAuth2 with Spring Security

## Scope
Modern Spring Security uses **OAuth 2.0 / OpenID Connect** as first-class patterns for:
- **Resource Server**: APIs that validate JWT access tokens (Bearer).
- **OAuth2 Client**: apps that log users in via an external IdP (authorization code flow).
- **Authorization Server**: specialized; many teams use Keycloak, Auth0, Okta, Entra ID instead.

## Resource Server (JWT) — Conceptual Setup
Dependencies (Maven coordinates illustrative):

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>
```

Properties commonly include issuer metadata:

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://your-idp/realms/your-realm
```

### Securing Endpoints
Use `SecurityFilterChain` beans (`HttpSecurity`) to authorize requests:
- `requestMatchers("/public/**").permitAll()`
- `anyRequest().authenticated()`

Map JWT claims to authorities with `JwtAuthenticationConverter` when roles arrive as nested JSON (`realm_access.roles`, `scp`, etc.).

## OAuth2 Login (Client) — When to Use
For **server-rendered** apps or BFF (backend-for-frontend) patterns that exchange a code at the IdP and maintain a session. Use `spring-boot-starter-oauth2-client` and configure `client-id`, `client-secret`, `scope`, `redirect-uri`.

## Testing Tips
- Use **WireMock** or Testcontainers-based IdP fixtures for integration tests.
- Prefer **`@WithMockJwt`** style utilities sparingly; validate claim mapping tests separately from business tests.

## Common Pitfalls
- Clock skew causing **`exp`** validation failures—sync NTP on nodes.
- Misconfigured **`aud` / `azp` / issuer`** validation—tighten only what your IdP guarantees.
- Logging **tokens**—never log full JWTs in production.

## Related Docs
- `12-Spring-Boot-Security.md` – broader security model.
- `87_JWT.md` (Java Concepts) – token structure fundamentals.
