# Spring Boot: SSL Bundles

**SSL bundles** (Spring Boot 3.1+) centralize keystore/truststore material so **multiple clients and servers** reuse the same certificate configuration without duplicating paths and passwords in many properties.

## Motivation
- One definition for **mTLS** to a partner API, reused by `RestClient`, JDBC, Kafka clients, etc.
- Easier **rotation**: update bundle wiring in one place (often backed by Kubernetes secrets).

## Declarative Pattern (Illustrative YAML)

```yaml
spring:
  ssl:
    bundle:
      jks:
        partner-api:
          keystore:
            location: "file:/etc/ssl/partner.p12"
            password: "${PARTNER_KEYSTORE_PASSWORD}"
          truststore:
            location: "file:/etc/ssl/partner-trust.p12"
            password: "${PARTNER_TRUSTSTORE_PASSWORD}"
```

Exact property keys and bundle types (`jks`, `pem`) depend on your Boot version—consult the **Spring Boot SSL** appendix for the release you use.

## Using a Bundle from Code
Boot exposes **`SslBundles`** bean; named bundles can be applied to client builders provided by each integration.

## Operational Notes
- Prefer **environment-specific** secrets (Vault, CSI driver) over plaintext files in images.
- Validate **certificate expiry** monitoring; bundle rotation still requires process.

## Related Topics
- Java: `125_TLS_Keystores_Keytool_and_Truststores.md`
- `60-Spring-Boot-RestClient.md`
- `12-Spring-Boot-Security.md`
