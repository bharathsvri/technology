# Spring Boot Error Pages

## Custom Error Pages

Spring Boot allows you to create custom error pages for different HTTP status codes to provide a better user experience.

## Static Error Pages

### HTML Error Pages

Create error pages in `src/main/resources/static/error/`:

```
resources/
└── static/
    └── error/
        ├── 404.html
        ├── 500.html
        └── error.html
```

### 404.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>404 - Page Not Found</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 50px;
        }
        h1 {
            color: #e74c3c;
        }
    </style>
</head>
<body>
    <h1>404 - Page Not Found</h1>
    <p>The page you are looking for does not exist.</p>
    <a href="/">Go to Home</a>
</body>
</html>
```

### 500.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>500 - Internal Server Error</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 50px;
        }
        h1 {
            color: #e74c3c;
        }
    </style>
</head>
<body>
    <h1>500 - Internal Server Error</h1>
    <p>Something went wrong on our end.</p>
    <a href="/">Go to Home</a>
</body>
</html>
```

## Thymeleaf Error Pages

### Template Error Pages

Create error pages in `src/main/resources/templates/error/`:

```
resources/
└── templates/
    └── error/
        ├── 404.html
        ├── 500.html
        └── error.html
```

### Thymeleaf Error Page

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Error</title>
</head>
<body>
    <h1 th:text="${status}">Error</h1>
    <p th:text="${error}">Error message</p>
    <p th:text="${message}">Detailed message</p>
    <p th:text="${path}">Request path</p>
    <a th:href="@{/}">Go to Home</a>
</body>
</html>
```

## Custom Error Controller

### Error Controller

```java
@Controller
public class CustomErrorController implements ErrorController {
    
    @RequestMapping("/error")
    public String handleError(HttpServletRequest request) {
        Object status = request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
        
        if (status != null) {
            Integer statusCode = Integer.valueOf(status.toString());
            
            if (statusCode == HttpStatus.NOT_FOUND.value()) {
                return "error/404";
            } else if (statusCode == HttpStatus.INTERNAL_SERVER_ERROR.value()) {
                return "error/500";
            }
        }
        
        return "error/error";
    }
}
```

## REST API Error Handling

### Error Response

```java
@RestController
public class CustomErrorController {
    
    @RequestMapping("/error")
    public ResponseEntity<ErrorResponse> handleError(HttpServletRequest request) {
        Object status = request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
        Object message = request.getAttribute(RequestDispatcher.ERROR_MESSAGE);
        
        ErrorResponse error = new ErrorResponse(
            status != null ? Integer.valueOf(status.toString()) : 500,
            message != null ? message.toString() : "An error occurred",
            request.getRequestURI(),
            System.currentTimeMillis()
        );
        
        return ResponseEntity.status(error.getStatus()).body(error);
    }
}
```

## Error Attributes

### Custom Error Attributes

```java
@Component
public class CustomErrorAttributes extends DefaultErrorAttributes {
    
    @Override
    public Map<String, Object> getErrorAttributes(WebRequest webRequest, 
                                                 ErrorAttributeOptions options) {
        Map<String, Object> errorAttributes = super.getErrorAttributes(webRequest, options);
        
        errorAttributes.put("timestamp", LocalDateTime.now());
        errorAttributes.put("status", errorAttributes.get("status"));
        errorAttributes.put("error", errorAttributes.get("error"));
        errorAttributes.put("message", errorAttributes.get("message"));
        errorAttributes.put("path", errorAttributes.get("path"));
        
        return errorAttributes;
    }
}
```

## Whitelabel Error Page

### Disable Whitelabel

```properties
server.error.whitelabel.enabled=false
```

## Best Practices

1. **User-Friendly Messages**: Provide clear error messages
2. **Consistent Design**: Use consistent error page design
3. **Include Navigation**: Include links to navigate away
4. **Log Errors**: Log errors for debugging
5. **Handle Different Status Codes**: Handle various HTTP status codes

## Summary

Spring Boot Error Pages:
- Custom error pages for better UX
- Support for static and template pages
- REST API error handling
- Essential for production applications

