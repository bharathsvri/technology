# TLS: Keystores, Truststores, and keytool

Java servers and HTTP clients use **KeyStores** (private keys + certificates for **your identity**) and **TrustStores** (CA certificates used to **verify peers**).

## keytool Quick Reference

```bash
# List a PKCS12 keystore
keytool -list -v -keystore server.p12 -storetype PKCS12

# Generate a self-signed cert for local dev (not for production)
keytool -genkeypair -alias server -keyalg RSA -keysize 2048 \
  -keystore server.p12 -storetype PKCS12 -validity 365 \
  -dname "CN=localhost"

# Import a CA or peer cert into a truststore
keytool -importcert -alias corp-ca -file corp-ca.crt \
  -keystore truststore.jks -storepass changeit
```

## PKCS12 vs JKS
- **PKCS12** (`.p12`, `.pfx`) is the modern default—interoperable and password-protected.
- **JKS** is legacy; avoid for new systems.

## Spring Boot Mapping (Conceptual)
- **`server.ssl.key-store*`** properties point at the **KeyStore** for embedded Tomcat.
- JVM system properties **`javax.net.ssl.trustStore`** (or Spring-specific SSL bundle properties in Boot 3.x) configure **trust** for outbound calls—prefer explicit trust over disabling verification.

## Never in Production
- **`TrustManager` that accepts all certificates**
- Shared passwords committed to git

## Related Topics
- `58_Security.md`
- `87_JWT.md` (transport security context)
