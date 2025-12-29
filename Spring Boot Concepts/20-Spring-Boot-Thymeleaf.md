# Spring Boot Thymeleaf

## What is Thymeleaf?

Thymeleaf is a modern server-side Java template engine for web and standalone environments. It's perfect for Spring Boot applications as it provides excellent integration with Spring MVC.

## Setting Up Thymeleaf

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### Configuration

```properties
# Thymeleaf Configuration
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
spring.thymeleaf.mode=HTML
spring.thymeleaf.cache=false
spring.thymeleaf.encoding=UTF-8
```

## Basic Thymeleaf Template

### Controller

```java
@Controller
public class HomeController {
    
    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("message", "Welcome to Spring Boot!");
        model.addAttribute("users", Arrays.asList("John", "Jane", "Bob"));
        return "index";
    }
}
```

### Template (index.html)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Home</title>
</head>
<body>
    <h1 th:text="${message}">Default Message</h1>
    <ul>
        <li th:each="user : ${users}" th:text="${user}">User</li>
    </ul>
</body>
</html>
```

## Thymeleaf Expressions

### Variable Expressions

```html
<p th:text="${user.name}">Name</p>
<p th:text="${user.email}">Email</p>
```

### Selection Expressions

```html
<div th:object="${user}">
    <p th:text="*{name}">Name</p>
    <p th:text="*{email}">Email</p>
</div>
```

### Message Expressions

```html
<p th:text="#{welcome.message}">Welcome</p>
```

### Link Expressions

```html
<a th:href="@{/users/{id}(id=${user.id})}">View User</a>
<a th:href="@{/users}">All Users</a>
```

## Common Thymeleaf Attributes

### th:text

```html
<p th:text="${message}">Default text</p>
```

### th:utext

```html
<p th:utext="${htmlContent}">Default HTML</p>
```

### th:each

```html
<ul>
    <li th:each="user : ${users}" th:text="${user.name}">User</li>
</ul>
```

### th:if and th:unless

```html
<div th:if="${user != null}">
    <p th:text="${user.name}">Name</p>
</div>

<div th:unless="${user == null}">
    <p>User exists</p>
</div>
```

### th:switch

```html
<div th:switch="${user.role}">
    <p th:case="'ADMIN'">Administrator</p>
    <p th:case="'USER'">Regular User</p>
    <p th:case="*">Unknown Role</p>
</div>
```

## Forms

### Form Binding

```html
<form th:action="@{/users}" th:object="${user}" method="post">
    <input type="text" th:field="*{name}" />
    <input type="email" th:field="*{email}" />
    <button type="submit">Submit</button>
</form>
```

### Controller

```java
@PostMapping("/users")
public String createUser(@ModelAttribute User user) {
    userService.createUser(user);
    return "redirect:/users";
}
```

## Layouts

### Using Thymeleaf Layout Dialect

#### Add Dependency

```xml
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
</dependency>
```

#### Layout Template (layout.html)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<head>
    <title layout:title-pattern="$CONTENT_TITLE - $LAYOUT_TITLE">Default Title</title>
</head>
<body>
    <header>
        <h1>My Application</h1>
    </header>
    <main layout:fragment="content">
        Default content
    </main>
    <footer>
        <p>&copy; 2024 My Application</p>
    </footer>
</body>
</html>
```

#### Page Template

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{layout}">
<head>
    <title>Home Page</title>
</head>
<body>
    <main layout:fragment="content">
        <h1>Welcome</h1>
        <p>This is the home page</p>
    </main>
</body>
</html>
```

## Fragments

### Defining Fragments

```html
<!-- fragments.html -->
<div th:fragment="header">
    <header>
        <nav>
            <a th:href="@{/}">Home</a>
            <a th:href="@{/users}">Users</a>
        </nav>
    </header>
</div>
```

### Using Fragments

```html
<div th:replace="~{fragments :: header}"></div>
```

## Internationalization

### Messages File (messages.properties)

```properties
welcome.message=Welcome to our application
user.name=Name
user.email=Email
```

### Using in Template

```html
<h1 th:text="#{welcome.message}">Welcome</h1>
<label th:text="#{user.name}">Name</label>
```

## Security Integration

### Spring Security with Thymeleaf

```html
<div sec:authorize="isAuthenticated()">
    <p>Welcome, <span sec:authentication="name">User</span></p>
</div>

<div sec:authorize="hasRole('ADMIN')">
    <a th:href="@{/admin}">Admin Panel</a>
</div>
```

## Complete Example

### Controller

```java
@Controller
@RequestMapping("/users")
public class UserController {
    
    private final UserService userService;
    
    @GetMapping
    public String getAllUsers(Model model) {
        model.addAttribute("users", userService.getAllUsers());
        return "users/list";
    }
    
    @GetMapping("/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        model.addAttribute("user", userService.getUserById(id));
        return "users/detail";
    }
    
    @GetMapping("/new")
    public String showCreateForm(Model model) {
        model.addAttribute("user", new User());
        return "users/form";
    }
    
    @PostMapping
    public String createUser(@ModelAttribute User user) {
        userService.createUser(user);
        return "redirect:/users";
    }
}
```

### Template (users/list.html)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Users</title>
</head>
<body>
    <h1>Users</h1>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Email</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr th:each="user : ${users}">
                <td th:text="${user.name}">Name</td>
                <td th:text="${user.email}">Email</td>
                <td>
                    <a th:href="@{/users/{id}(id=${user.id})}">View</a>
                </td>
            </tr>
        </tbody>
    </table>
    <a th:href="@{/users/new}">Add New User</a>
</body>
</html>
```

## Best Practices

1. **Use Layouts**: Use layout templates for consistency
2. **Use Fragments**: Reuse common components
3. **Validate Input**: Always validate form input
4. **Use i18n**: Support multiple languages
5. **Optimize Templates**: Cache templates in production
6. **Security**: Use Spring Security integration
7. **Error Handling**: Handle errors gracefully

## Summary

Spring Boot Thymeleaf:
- Easy to integrate with Spring Boot
- Supports server-side rendering
- Provides excellent Spring integration
- Supports layouts and fragments
- Essential for traditional web applications

