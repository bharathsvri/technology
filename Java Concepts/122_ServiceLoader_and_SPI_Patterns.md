# ServiceLoader and SPI Patterns

The **Service Provider Interface (SPI)** pattern lets libraries discover implementations at **runtime** from the classpath or module path without hard-coding concrete types.

## `ServiceLoader` Basics

```java
import java.util.ServiceLoader;

ServiceLoader<PaymentGateway> loader = ServiceLoader.load(PaymentGateway.class);
for (PaymentGateway gateway : loader) {
    // use gateway
}
```

- Implementations declare a provider configuration file under **`META-INF/services/<fully-qualified-interface-name>`** listing implementation class names (one per line).
- On the **module path**, `module-info.java` may use **`provides X with Y;`** instead of or in addition to the `META-INF/services` file.

## When SPI Fits
- **Pluggable** algorithms (compression codecs, payment providers).
- **Framework extensions** loaded from optional JARs.

## Pitfalls
- **ClassLoader** choice matters in EE containers and OSGi—use `ServiceLoader.load(iface, classLoader)` intentionally.
- **Lazy vs eager**: iterator materialization can hide initialization failures until first use.
- **Security**: loading arbitrary classes from SPI files can be risky if the classpath is untrusted.

## Related Topics
- `43_Module_System.md`
- `52_ClassLoader.md`
