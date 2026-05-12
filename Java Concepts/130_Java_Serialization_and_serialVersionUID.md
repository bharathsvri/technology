# Java Serialization (`Serializable`) and Versioning

**`java.io.Serializable`** marks classes whose instances can be **byte-streamed**—still common in **legacy** remoting and **session** clustering, but **JSON/Protobuf** dominates new APIs.

---

## `serialVersionUID`

If not declared, JVM generates one from **class shape**—any **field change** can cause **`InvalidClassException`** on deserialize.

**Practice**: declare **`private static final long serialVersionUID = 1L;`** intentionally bump when making **compatible** evolution (rare art).

---

## Pitfalls

- **Security**: deserialization of **untrusted** bytes is a **remote code execution** vector—**avoid** or use **`ObjectInputFilter`** / allowlists.
- **Brittle** across JVM versions and **private** implementation details leak into wire format.
- **Performance**: reflection-heavy vs schema-based formats.

---

## Alternatives to mention

**Jackson**, **Protocol Buffers**, **Avro** for cross-language, evolvable payloads.

---

## Related

- [Jackson Advanced](100_Jackson_Advanced.md)
- [Networking](37_Networking.md)
