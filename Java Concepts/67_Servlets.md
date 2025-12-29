# Servlets

## What are Servlets?
Servlets are Java programs that run on a web server and handle HTTP requests and responses.

## Basic Servlet
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, 
                        HttpServletResponse response) 
            throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h1>Hello Servlet</h1>");
        out.println("</body></html>");
    }
}
```

## Servlet Lifecycle
```java
public class LifecycleServlet extends HttpServlet {
    @Override
    public void init() throws ServletException {
        // Called once when servlet is first loaded
        System.out.println("Servlet initialized");
    }
    
    @Override
    protected void service(HttpServletRequest request, 
                          HttpServletResponse response) 
            throws ServletException, IOException {
        // Called for each request
        super.service(request, response);
    }
    
    @Override
    public void destroy() {
        // Called when servlet is unloaded
        System.out.println("Servlet destroyed");
    }
}
```

## Handling HTTP Methods
```java
@WebServlet("/api/users")
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, 
                        HttpServletResponse response) 
            throws ServletException, IOException {
        // Handle GET request
    }
    
    @Override
    protected void doPost(HttpServletRequest request, 
                         HttpServletResponse response) 
            throws ServletException, IOException {
        // Handle POST request
    }
    
    @Override
    protected void doPut(HttpServletRequest request, 
                        HttpServletResponse response) 
            throws ServletException, IOException {
        // Handle PUT request
    }
    
    @Override
    protected void doDelete(HttpServletRequest request, 
                           HttpServletResponse response) 
            throws ServletException, IOException {
        // Handle DELETE request
    }
}
```

## Request Parameters
```java
@WebServlet("/search")
public class SearchServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, 
                        HttpServletResponse response) 
            throws ServletException, IOException {
        String query = request.getParameter("q");
        String[] categories = request.getParameterValues("category");
        
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("Query: " + query);
    }
}
```

## Session Management
```java
@WebServlet("/session")
public class SessionServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, 
                        HttpServletResponse response) 
            throws ServletException, IOException {
        HttpSession session = request.getSession();
        
        // Set session attribute
        session.setAttribute("username", "john");
        
        // Get session attribute
        String username = (String) session.getAttribute("username");
        
        // Invalidate session
        // session.invalidate();
    }
}
```

## Filters
```java
@WebFilter("/*")
public class LoggingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) {
        // Initialization
    }
    
    @Override
    public void doFilter(ServletRequest request, 
                        ServletResponse response, 
                        FilterChain chain) 
            throws IOException, ServletException {
        System.out.println("Request: " + 
            ((HttpServletRequest) request).getRequestURI());
        chain.doFilter(request, response);
    }
    
    @Override
    public void destroy() {
        // Cleanup
    }
}
```

## Best Practices
1. Use annotations for configuration
2. Handle exceptions properly
3. Use appropriate HTTP methods
4. Manage sessions efficiently
5. Use filters for cross-cutting concerns
6. Set proper content types
7. Handle character encoding
8. Use async servlets for long operations

