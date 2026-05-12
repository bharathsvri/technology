# View Resolution

For **server-rendered** HTML (Thymeleaf, FreeMarker, JSP), controller methods return **logical view names** (or `ModelAndView`). **`ViewResolver`** beans map those names to a **`View`** that renders the response.

---

## `ViewResolver` chain

Spring consults **ordered** `ViewResolver` beans; first non-null `View` wins.

Common resolvers:

| Resolver | Example |
|----------|---------|
| **`InternalResourceViewResolver`** | JSP under `/WEB-INF/views/` |
| **`ThymeleafViewResolver`** | `.html` templates |
| **`ContentNegotiatingViewResolver`** | Chooses JSON vs HTML by `Accept` |

---

## Return types

```java
@GetMapping("/hello")
public String hello(Model model) {
    model.addAttribute("name", "Spring");
    return "hello"; // → hello.html / hello.jsp depending on resolver
}
```

**`Model` / `ModelMap` / `@ModelAttribute`** populate template context.

---

## Redirect and forward prefixes

```java
return "redirect:/orders/123";  // 302 to client
return "forward:/internal";     // server-side forward to another handler
```

---

## REST vs MVC in same app

You can mix `@RestController` APIs and `@Controller` page routes; keep **URL namespaces** clear (`/api/**` vs `/**`).

---

## Error views

`BasicErrorController` (Boot) or custom **`ErrorController`** / template `error.html` for whitelabel pages.

---

## Next

- [REST with Spring MVC](14-REST-with-Spring-MVC.md)
