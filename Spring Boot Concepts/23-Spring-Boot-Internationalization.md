# Spring Boot Internationalization (i18n)

## What is Internationalization?

Internationalization (i18n) is the process of designing applications that can be adapted to various languages and regions without engineering changes.

## Setting Up i18n

### Configuration

```properties
# Internationalization
spring.web.locale=en
spring.web.locale-resolver=fixed
spring.messages.basename=messages
spring.messages.encoding=UTF-8
```

### Message Files

**messages.properties** (default):
```properties
welcome.message=Welcome
greeting=Hello, {0}!
user.name=Name
user.email=Email
```

**messages_fr.properties** (French):
```properties
welcome.message=Bienvenue
greeting=Bonjour, {0}!
user.name=Nom
user.email=Email
```

**messages_es.properties** (Spanish):
```properties
welcome.message=Bienvenido
greeting=¡Hola, {0}!
user.name=Nombre
user.email=Correo electrónico
```

## Locale Resolution

### AcceptHeaderLocaleResolver

```java
@Configuration
public class LocaleConfig {
    
    @Bean
    public LocaleResolver localeResolver() {
        AcceptHeaderLocaleResolver resolver = new AcceptHeaderLocaleResolver();
        resolver.setDefaultLocale(Locale.US);
        return resolver;
    }
}
```

### SessionLocaleResolver

```java
@Bean
public LocaleResolver localeResolver() {
    SessionLocaleResolver resolver = new SessionLocaleResolver();
    resolver.setDefaultLocale(Locale.US);
    return resolver;
}
```

### CookieLocaleResolver

```java
@Bean
public LocaleResolver localeResolver() {
    CookieLocaleResolver resolver = new CookieLocaleResolver();
    resolver.setDefaultLocale(Locale.US);
    resolver.setCookieName("locale");
    resolver.setCookieMaxAge(3600);
    return resolver;
}
```

## Using Messages

### In Controllers

```java
@RestController
public class HomeController {
    
    @Autowired
    private MessageSource messageSource;
    
    @GetMapping("/")
    public String home(Locale locale) {
        return messageSource.getMessage("welcome.message", null, locale);
    }
    
    @GetMapping("/greeting")
    public String greeting(@RequestParam String name, Locale locale) {
        return messageSource.getMessage("greeting", new Object[]{name}, locale);
    }
}
```

### In Thymeleaf Templates

```html
<h1 th:text="#{welcome.message}">Welcome</h1>
<p th:text="#{greeting(${name})}">Hello</p>
```

## Changing Locale

### Locale Change Interceptor

```java
@Configuration
public class LocaleConfig implements WebMvcConfigurer {
    
    @Bean
    public LocaleResolver localeResolver() {
        SessionLocaleResolver resolver = new SessionLocaleResolver();
        resolver.setDefaultLocale(Locale.US);
        return resolver;
    }
    
    @Bean
    public LocaleChangeInterceptor localeChangeInterceptor() {
        LocaleChangeInterceptor interceptor = new LocaleChangeInterceptor();
        interceptor.setParamName("lang");
        return interceptor;
    }
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(localeChangeInterceptor());
    }
}
```

### Changing Locale via URL

```java
@GetMapping("/change-locale")
public String changeLocale(@RequestParam String lang, HttpServletRequest request, 
                         HttpServletResponse response) {
    Locale locale = new Locale(lang);
    localeResolver.setLocale(request, response, locale);
    return "redirect:/";
}
```

## Complete Example

### Configuration

```java
@Configuration
public class LocaleConfig implements WebMvcConfigurer {
    
    @Bean
    public LocaleResolver localeResolver() {
        SessionLocaleResolver resolver = new SessionLocaleResolver();
        resolver.setDefaultLocale(Locale.US);
        return resolver;
    }
    
    @Bean
    public LocaleChangeInterceptor localeChangeInterceptor() {
        LocaleChangeInterceptor interceptor = new LocaleChangeInterceptor();
        interceptor.setParamName("lang");
        return interceptor;
    }
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(localeChangeInterceptor());
    }
}
```

### Controller

```java
@Controller
public class HomeController {
    
    @Autowired
    private MessageSource messageSource;
    
    @GetMapping("/")
    public String home(Model model, Locale locale) {
        model.addAttribute("welcome", 
            messageSource.getMessage("welcome.message", null, locale));
        return "index";
    }
}
```

## Best Practices

1. **Use Message Keys**: Use meaningful message keys
2. **Externalize Messages**: Keep messages in properties files
3. **Support Multiple Locales**: Support all required locales
4. **Test Translations**: Test all supported locales
5. **Handle Missing Messages**: Provide fallback messages

## Summary

Spring Boot Internationalization:
- Easy to configure
- Supports multiple languages
- Flexible locale resolution
- Essential for global applications

