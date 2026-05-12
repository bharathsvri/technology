# Spring Security Overview

**Spring Security** is a servlet **filter chain** (and reactive equivalent) that provides **authentication** (who you are) and **authorization** (what you may do). It integrates deeply with Spring MVC, WebFlux, OAuth2, SAML, LDAP, etc.

---

## Filter chain mental model

1. Request enters **`DelegatingFilterProxy`** (registered in servlet container).
2. Delegates to **`FilterChainProxy`** which runs an ordered list of **`SecurityFilter`** implementations.
3. Filters build an **`Authentication`** object and populate **`SecurityContextHolder`**.
4. **`AuthorizationManager`** / `AccessDecisionManager` (legacy) decide access to resources.

---

## Core types

| Type | Role |
|------|------|
| **`Authentication`** | Credentials + principal + authorities |
| **`SecurityContextHolder`** | Thread-local (or reactive context) holder |
| **`UserDetailsService`** | Load user for username/password login |
| **`PasswordEncoder`** | Hash and verify passwords (`bcrypt`, etc.) |

---

## Configuration styles

Modern Spring Security 6.x uses **`SecurityFilterChain` `@Bean`** with **`HttpSecurity`** DSL:

```java
@Bean
SecurityFilterChain chain(HttpSecurity http) throws Exception {
    return http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/public/**").permitAll()
            .anyRequest().authenticated())
        .httpBasic(Customizer.withDefaults())
        .build();
}
```

---

## Method security

`@PreAuthorize("hasRole('ADMIN')")` on service methods — requires **`@EnableMethodSecurity`**.

---

## CSRF

Browser cookie-based sessions need **CSRF tokens** for state-changing requests; APIs using **Bearer tokens** often disable CSRF for those endpoints—do so **consciously**.

---

## Further reading

See **`Spring Boot Concepts`** OAuth2 and Security docs in this repo for Boot-specific property names.

---

## Next

- [Spring Transactions](19-Spring-Transactions.md)
