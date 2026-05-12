# HTTP Message Converters

**`HttpMessageConverter<T>`** converts between HTTP message bodies and Java types. On the **inbound** side, the `Content-Type` request header selects a converter; on **outbound**, `Accept` and the method’s return type matter.

---

## Jackson (JSON)

`MappingJackson2HttpMessageConverter` (Jackson 3 naming may differ) handles `application/json` for:

- `@RequestBody MyDto dto`
- `@ResponseBody` / `@RestController` return types

Customize `ObjectMapper` beans for date formats, property naming, modules.

---

## String and byte[]

`StringHttpMessageConverter`, `ByteArrayHttpMessageConverter` for raw payloads.

---

## XML

`Jaxb2RootElementHttpMessageConverter` when JAXB is on classpath (less common in greenfield JSON APIs).

---

## Registering custom converters

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        converters.add(0, new MyProtobufConverter());
    }
}
```

Order matters: **first matching** converter wins for a given media type.

---

## `HttpMessageNotReadableException`

Thrown when body cannot be deserialized (malformed JSON, unknown enum). Map to **400** in advice.

---

## Large payloads

Configure Jackson **streaming** or increase limits carefully; for huge uploads consider **multipart** + streaming parsers.

---

## Next

- [Validation with Spring](17-Validation-with-Spring.md)
