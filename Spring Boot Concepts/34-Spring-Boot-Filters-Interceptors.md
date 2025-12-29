# Spring Boot Filters and Interceptors

## What are Filters and Interceptors?

Filters and Interceptors allow you to intercept and process HTTP requests and responses. Filters work at the servlet level, while Interceptors work at the Spring MVC level.

## Filters

### Creating a Filter

```java
@Component
@Order(1)
public class LoggingFilter implements Filter {
    
    private static final Logger logger = LoggerFactory.getLogger(LoggingFilter.class);
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        
        long startTime = System.currentTimeMillis();
        logger.info("Request: {} {}", httpRequest.getMethod(), httpRequest.getRequestURI());
        
        chain.doFilter(request, response);
        
        long duration = System.currentTimeMillis() - startTime;
        logger.info("Response time: {}ms", duration);
    }
}
```

### Authentication Filter

```java
@Component
@Order(2)
public class AuthenticationFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String token = httpRequest.getHeader("Authorization");
        
        if (token == null || !isValidToken(token)) {
            HttpServletResponse httpResponse = (HttpServletResponse) response;
            httpResponse.setStatus(HttpStatus.UNAUTHORIZED.value());
            return;
        }
        
        chain.doFilter(request, response);
    }
    
    private boolean isValidToken(String token) {
        // Token validation logic
        return true;
    }
}
```

### CORS Filter

```java
@Component
public class CorsFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        
        httpResponse.setHeader("Access-Control-Allow-Origin", "*");
        httpResponse.setHeader("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS");
        httpResponse.setHeader("Access-Control-Allow-Headers", "Content-Type, Authorization");
        
        if ("OPTIONS".equalsIgnoreCase(httpRequest.getMethod())) {
            httpResponse.setStatus(HttpStatus.OK.value());
            return;
        }
        
        chain.doFilter(request, response);
    }
}
```

## Interceptors

### Creating an Interceptor

```java
@Component
public class LoggingInterceptor implements HandlerInterceptor {
    
    private static final Logger logger = LoggerFactory.getLogger(LoggingInterceptor.class);
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                           Object handler) throws Exception {
        logger.info("Before handler: {} {}", request.getMethod(), request.getRequestURI());
        request.setAttribute("startTime", System.currentTimeMillis());
        return true;
    }
    
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, 
                          Object handler, ModelAndView modelAndView) throws Exception {
        long startTime = (Long) request.getAttribute("startTime");
        long duration = System.currentTimeMillis() - startTime;
        logger.info("After handler: {}ms", duration);
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
                               Object handler, Exception ex) throws Exception {
        if (ex != null) {
            logger.error("Exception in handler", ex);
        }
    }
}
```

### Registering Interceptors

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    private final LoggingInterceptor loggingInterceptor;
    
    public WebConfig(LoggingInterceptor loggingInterceptor) {
        this.loggingInterceptor = loggingInterceptor;
    }
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loggingInterceptor)
            .addPathPatterns("/api/**")
            .excludePathPatterns("/api/public/**");
    }
}
```

## Request/Response Modification

### Request Wrapper

```java
public class CustomRequestWrapper extends HttpServletRequestWrapper {
    
    private final String body;
    
    public CustomRequestWrapper(HttpServletRequest request) throws IOException {
        super(request);
        body = request.getReader().lines()
            .collect(Collectors.joining(System.lineSeparator()));
    }
    
    @Override
    public ServletInputStream getInputStream() throws IOException {
        ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(body.getBytes());
        return new ServletInputStream() {
            @Override
            public int read() throws IOException {
                return byteArrayInputStream.read();
            }
            
            @Override
            public boolean isFinished() {
                return byteArrayInputStream.available() == 0;
            }
            
            @Override
            public boolean isReady() {
                return true;
            }
            
            @Override
            public void setReadListener(ReadListener listener) {
            }
        };
    }
    
    public String getBody() {
        return body;
    }
}
```

### Using Request Wrapper in Filter

```java
@Component
public class RequestLoggingFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        CustomRequestWrapper wrappedRequest = new CustomRequestWrapper((HttpServletRequest) request);
        logger.info("Request body: {}", wrappedRequest.getBody());
        chain.doFilter(wrappedRequest, response);
    }
}
```

## Best Practices

1. **Use Appropriate Order**: Set filter/interceptor order correctly
2. **Handle Exceptions**: Handle exceptions properly
3. **Keep Fast**: Keep filters/interceptors fast
4. **Use for Cross-Cutting Concerns**: Use for logging, security, etc.
5. **Document Purpose**: Document what each filter/interceptor does

## Summary

Spring Boot Filters and Interceptors:
- Intercept HTTP requests and responses
- Useful for logging, security, CORS
- Filters work at servlet level
- Interceptors work at Spring MVC level
- Essential for cross-cutting concerns

