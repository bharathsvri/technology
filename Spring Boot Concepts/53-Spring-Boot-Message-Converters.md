# Spring Boot Message Converters

## What are Message Converters?

Message converters are responsible for converting HTTP request and response bodies to and from Java objects. Spring Boot provides several built-in converters and allows custom converters.

## Built-in Message Converters

### Default Converters

Spring Boot automatically configures:
- `MappingJackson2HttpMessageConverter` - JSON
- `MappingJackson2XmlHttpMessageConverter` - XML
- `StringHttpMessageConverter` - String
- `ByteArrayHttpMessageConverter` - Byte arrays
- `ResourceHttpMessageConverter` - Resources

## Custom Message Converters

### JSON Converter Configuration

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        Jackson2ObjectMapperBuilder builder = new Jackson2ObjectMapperBuilder()
            .indentOutput(true)
            .dateFormat(new SimpleDateFormat("yyyy-MM-dd"))
            .modulesToInstall(new JavaTimeModule());
        
        converters.add(new MappingJackson2HttpMessageConverter(builder.build()));
    }
}
```

### Custom JSON Converter

```java
public class CustomJsonMessageConverter extends AbstractHttpMessageConverter<Object> {
    
    private final ObjectMapper objectMapper;
    
    public CustomJsonMessageConverter() {
        super(MediaType.APPLICATION_JSON);
        this.objectMapper = new ObjectMapper();
        this.objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    }
    
    @Override
    protected boolean supports(Class<?> clazz) {
        return true;
    }
    
    @Override
    protected Object readInternal(Class<?> clazz, HttpInputMessage inputMessage) 
            throws IOException, HttpMessageNotReadableException {
        return objectMapper.readValue(inputMessage.getBody(), clazz);
    }
    
    @Override
    protected void writeInternal(Object object, HttpOutputMessage outputMessage) 
            throws IOException, HttpMessageNotWritableException {
        objectMapper.writeValue(outputMessage.getBody(), object);
    }
}
```

## XML Converter

### XML Converter Configuration

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Bean
    public MappingJackson2XmlHttpMessageConverter xmlMessageConverter() {
        return new MappingJackson2XmlHttpMessageConverter(
            Jackson2ObjectMapperBuilder.xml()
                .indentOutput(true)
                .build()
        );
    }
}
```

## CSV Converter

### CSV Message Converter

```java
public class CsvMessageConverter extends AbstractHttpMessageConverter<Object> {
    
    public CsvMessageConverter() {
        super(new MediaType("text", "csv"));
    }
    
    @Override
    protected boolean supports(Class<?> clazz) {
        return List.class.isAssignableFrom(clazz);
    }
    
    @Override
    protected Object readInternal(Class<?> clazz, HttpInputMessage inputMessage) 
            throws IOException, HttpMessageNotReadableException {
        // CSV reading logic
        return null;
    }
    
    @Override
    protected void writeInternal(Object object, HttpOutputMessage outputMessage) 
            extends IOException, HttpMessageNotWritableException {
        List<?> list = (List<?>) object;
        try (PrintWriter writer = new PrintWriter(outputMessage.getBody())) {
            // Write CSV header
            // Write CSV rows
        }
    }
}
```

## Registering Converters

### Register Custom Converter

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        converters.add(new CustomJsonMessageConverter());
        converters.add(new CsvMessageConverter());
    }
    
    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        // Modify existing converters
    }
}
```

## Content Negotiation

### Using Content Negotiation

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(produces = {MediaType.APPLICATION_JSON_VALUE, "text/csv"})
    public ResponseEntity<?> getAllUsers(HttpServletRequest request) {
        List<User> users = userService.getAllUsers();
        
        String acceptHeader = request.getHeader("Accept");
        if ("text/csv".equals(acceptHeader)) {
            return ResponseEntity.ok()
                .contentType(MediaType.parseMediaType("text/csv"))
                .body(convertToCsv(users));
        }
        
        return ResponseEntity.ok(users);
    }
}
```

## Best Practices

1. **Use Appropriate Converters**: Choose right converter for content type
2. **Handle Errors**: Handle conversion errors gracefully
3. **Optimize Performance**: Optimize converters for performance
4. **Support Multiple Formats**: Support multiple content types
5. **Test Converters**: Test custom converters thoroughly

## Summary

Spring Boot Message Converters:
- Convert between HTTP and Java objects
- Support multiple content types
- Custom converters
- Essential for REST APIs

