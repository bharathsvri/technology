# Spring Boot Content Negotiation

## What is Content Negotiation?

Content negotiation is the process of selecting the best representation of a resource based on the client's preferences and server capabilities.

## Configuration

### Content Negotiation Config

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        configurer
            .favorParameter(true)
            .parameterName("format")
            .ignoreAcceptHeader(false)
            .defaultContentType(MediaType.APPLICATION_JSON)
            .mediaType("json", MediaType.APPLICATION_JSON)
            .mediaType("xml", MediaType.APPLICATION_XML);
    }
}
```

### Properties Configuration

```properties
spring.mvc.contentnegotiation.favor-parameter=true
spring.mvc.contentnegotiation.parameter-name=format
spring.mvc.contentnegotiation.media-types.json=application/json
spring.mvc.contentnegotiation.media-types.xml=application/xml
```

## Multiple Content Types

### JSON and XML Support

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public ResponseEntity<List<User>> getAllUsers() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
    
    @GetMapping(value = "/{id}", 
                produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
}
```

## Custom Message Converters

### XML Converter

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        converters.add(new MappingJackson2XmlHttpMessageConverter());
        converters.add(new MappingJackson2HttpMessageConverter());
    }
}
```

### Custom Converter

```java
public class CustomMessageConverter extends AbstractHttpMessageConverter<User> {
    
    public CustomMessageConverter() {
        super(new MediaType("application", "vnd.custom+json"));
    }
    
    @Override
    protected boolean supports(Class<?> clazz) {
        return User.class.isAssignableFrom(clazz);
    }
    
    @Override
    protected User readInternal(Class<? extends User> clazz, 
                               HttpInputMessage inputMessage) throws IOException {
        // Custom deserialization
        return null;
    }
    
    @Override
    protected void writeInternal(User user, HttpOutputMessage outputMessage) 
            throws IOException {
        // Custom serialization
    }
}
```

## Accept Header

### Using Accept Header

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<List<User>> getUsersJson() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
    
    @GetMapping(produces = MediaType.APPLICATION_XML_VALUE)
    public ResponseEntity<List<User>> getUsersXml() {
        return ResponseEntity.ok(userService.getAllUsers());
    }
}
```

## Request Parameter

### Format Parameter

```java
@GetMapping(value = "/api/users", params = "format=json")
public ResponseEntity<List<User>> getUsersJson() {
    return ResponseEntity.ok(userService.getAllUsers());
}

@GetMapping(value = "/api/users", params = "format=xml")
public ResponseEntity<List<User>> getUsersXml() {
    return ResponseEntity.ok(userService.getAllUsers());
}
```

## Best Practices

1. **Support Multiple Formats**: Support JSON and XML
2. **Default Format**: Set sensible default format
3. **Custom Converters**: Use custom converters when needed
4. **Document Formats**: Document supported formats
5. **Version Headers**: Use version headers for content negotiation

## Summary

Spring Boot Content Negotiation:
- Supports multiple content types
- Flexible negotiation strategies
- Custom message converters
- Essential for RESTful APIs

