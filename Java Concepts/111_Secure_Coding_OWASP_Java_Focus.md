# Secure Coding: OWASP-Oriented Focus for Java

This complements `58_Security.md` with **application-level** pitfalls teams most often miss.

## 1. Injection (A03:2021 – Injection)
- **SQL/JPQL**: always use **parameter binding**; never concatenate user input into queries.
- **OS commands**: avoid `Runtime.exec` with user-controlled strings; use `ProcessBuilder` with a fixed command list if unavoidable.
- **LDAP / XPath / SpEL**: treat untrusted input as hostile; validate and parameterize.

## 2. Broken Authentication / Session Management
- Prefer proven frameworks (**Spring Security**) over custom crypto.
- Use **strong password hashing** (e.g., bcrypt / Argon2 via modern Spring delegating encoder patterns).
- Enforce **MFA** at the identity provider for human users where possible.

## 3. Sensitive Data Exposure
- Never log **tokens, passwords, PAN, health data**—scrub structured logs.
- Use **TLS** everywhere externally; pin internal mTLS where appropriate.
- Encrypt secrets using a **KMS/Vault**; avoid `.properties` for production secrets.

## 4. XML External Entity (XXE)
When parsing XML (`DocumentBuilderFactory`, JAXB legacy paths), disable external entities:

```java
factory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
```

Prefer safe defaults from your parser library’s hardened APIs.

## 5. Deserialization Gadgets
- Avoid deserializing **untrusted binary Java serialization**.
- For JSON, keep **polymorphic types** constrained (Jackson `@JsonTypeInfo` with whitelisting) and update libraries regularly.

## 6. Mass Assignment and Over-Posting
In REST APIs, use **DTOs** for input rather than binding directly to JPA entities; whitelist fields accepted from clients.

## 7. Path Traversal / File Uploads
- Normalize paths; ensure resolved path **stays under** an allowed root directory.
- Validate **content type and size**; scan if policy requires; store outside web root.

## 8. SSRF
Do not let user input fully control backend HTTP/JDBC URLs. Use **allowlists**, static hosts, or service discovery.

## 9. Dependencies and Supply Chain
- Run **`mvn dependency:tree` / `./gradlew dependencies`** in CI; fail on critical CVEs (OWASP Dependency-Check, Snyk, GitHub Dependabot).
- Prefer **minimal** dependencies and keep patch versions current.

## 10. Security Headers (Web Apps)
When serving HTML or APIs behind browsers, configure headers (often via gateway or Spring Security):
- `Content-Security-Policy`
- `X-Content-Type-Options: nosniff`
- `Strict-Transport-Security`
- `Frame-Options` / `frame-ancestors`

## Practical Team Habits
- **Threat modeling** for new features touching auth, files, payments, or admin tools.
- **Security code review** checklist tied to OWASP Top 10 categories.
- **Pen tests** plus **DAST** for public endpoints.

## References
- OWASP Top 10 (latest edition).
- OWASP Cheat Sheet Series (Java/JWT/REST/XXE/Deserialization).
