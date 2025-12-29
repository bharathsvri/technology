# Spring Boot View Resolvers

## What are View Resolvers?

View resolvers map view names returned by controllers to actual view implementations. Spring Boot supports various view technologies through view resolvers.

## Thymeleaf View Resolver

### Configuration

```java
@Configuration
public class ThymeleafConfig {
    
    @Bean
    public SpringTemplateEngine templateEngine() {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver());
        return templateEngine;
    }
    
    @Bean
    public ITemplateResolver templateResolver() {
        SpringResourceTemplateResolver resolver = new SpringResourceTemplateResolver();
        resolver.setPrefix("classpath:/templates/");
        resolver.setSuffix(".html");
        resolver.setTemplateMode(TemplateMode.HTML);
        resolver.setCacheable(false);
        return resolver;
    }
    
    @Bean
    public ThymeleafViewResolver viewResolver() {
        ThymeleafViewResolver resolver = new ThymeleafViewResolver();
        resolver.setTemplateEngine(templateEngine());
        resolver.setCharacterEncoding("UTF-8");
        return resolver;
    }
}
```

## Multiple View Resolvers

### Configuring Multiple Resolvers

```java
@Configuration
public class ViewResolverConfig implements WebMvcConfigurer {
    
    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        // Thymeleaf
        registry.viewResolver(thymeleafViewResolver());
        
        // JSP (if needed)
        // registry.jsp("/WEB-INF/views/", ".jsp");
        
        // Content Negotiation
        registry.enableContentNegotiation(new MappingJackson2JsonView());
    }
    
    @Bean
    public ThymeleafViewResolver thymeleafViewResolver() {
        ThymeleafViewResolver resolver = new ThymeleafViewResolver();
        resolver.setTemplateEngine(templateEngine());
        resolver.setOrder(1);
        return resolver;
    }
}
```

## JSON View Resolver

### JSON View

```java
@Configuration
public class ViewResolverConfig implements WebMvcConfigurer {
    
    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.enableContentNegotiation(new MappingJackson2JsonView());
    }
}
```

### Using JSON View

```java
@Controller
public class UserController {
    
    @GetMapping(value = "/api/users", produces = MediaType.APPLICATION_JSON_VALUE)
    public ModelAndView getUsersJson() {
        List<User> users = userService.getAllUsers();
        ModelAndView mav = new ModelAndView(new MappingJackson2JsonView());
        mav.addObject("users", users);
        return mav;
    }
}
```

## Custom View Resolver

### Custom View Resolver

```java
public class CustomViewResolver implements ViewResolver {
    
    @Override
    public View resolveViewName(String viewName, Locale locale) throws Exception {
        if (viewName.startsWith("custom:")) {
            return new CustomView(viewName.substring(7));
        }
        return null;
    }
}

public class CustomView implements View {
    
    private final String viewName;
    
    public CustomView(String viewName) {
        this.viewName = viewName;
    }
    
    @Override
    public void render(Map<String, ?> model, HttpServletRequest request, 
                      HttpServletResponse response) throws Exception {
        // Custom rendering logic
        response.getWriter().write("Custom view: " + viewName);
    }
    
    @Override
    public String getContentType() {
        return MediaType.TEXT_HTML_VALUE;
    }
}
```

## Best Practices

1. **Use Appropriate Resolvers**: Choose right resolver for view technology
2. **Order Resolvers**: Set proper order for multiple resolvers
3. **Cache Views**: Enable view caching in production
4. **Handle Errors**: Handle view resolution errors
5. **Test Views**: Test view resolution

## Summary

Spring Boot View Resolvers:
- Map view names to implementations
- Support multiple view technologies
- Custom view resolvers
- Essential for MVC applications

