# Spring Boot Resource Handling

## Static Resource Handling

Spring Boot automatically serves static resources from specific locations in your application.

## Default Resource Locations

### Static Resource Locations

Spring Boot looks for static resources in:
1. `/static`
2. `/public`
3. `/resources`
4. `/META-INF/resources`

### Example Structure

```
src/main/resources/
├── static/
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── app.js
│   └── images/
│       └── logo.png
└── templates/
    └── index.html
```

## Custom Resource Locations

### Configuration

```java
@Configuration
public class ResourceConfig implements WebMvcConfigurer {
    
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/files/**")
            .addResourceLocations("file:/path/to/files/");
        
        registry.addResourceHandler("/custom/**")
            .addResourceLocations("classpath:/custom/");
    }
}
```

### Properties Configuration

```properties
spring.web.resources.static-locations=classpath:/static/,classpath:/public/
spring.web.resources.chain.strategy.content.enabled=true
spring.web.resources.chain.strategy.content.paths=/**
```

## Resource Versioning

### Versioned Resources

```java
@Configuration
public class ResourceConfig implements WebMvcConfigurer {
    
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**")
            .addResourceLocations("classpath:/static/")
            .resourceChain(true)
            .addResolver(new VersionResourceResolver()
                .addVersionStrategy(new ContentVersionStrategy(), "/**"));
    }
}
```

## CDN Configuration

### Using CDN

```java
@Configuration
public class ResourceConfig implements WebMvcConfigurer {
    
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**")
            .addResourceLocations("classpath:/static/")
            .resourceChain(true)
            .addResolver(new CdnResourceResolver()
                .addCdn("https://cdn.example.com"));
    }
}
```

## Caching

### Cache Control

```java
@Configuration
public class ResourceConfig implements WebMvcConfigurer {
    
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**")
            .addResourceLocations("classpath:/static/")
            .setCachePeriod(3600)
            .resourceChain(true);
    }
}
```

## Best Practices

1. **Organize Resources**: Organize resources in logical folders
2. **Use CDN**: Use CDN for production
3. **Enable Caching**: Enable caching for static resources
4. **Minify Resources**: Minify CSS and JavaScript
5. **Version Resources**: Version resources for cache busting

## Summary

Spring Boot Resource Handling:
- Automatic static resource serving
- Custom resource locations
- Resource versioning
- CDN support
- Essential for web applications

