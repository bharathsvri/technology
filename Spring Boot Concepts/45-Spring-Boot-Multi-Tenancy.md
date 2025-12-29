# Spring Boot Multi-Tenancy

## What is Multi-Tenancy?

Multi-tenancy is an architecture where a single instance of an application serves multiple tenants (customers), with each tenant's data isolated and secure.

## Approaches to Multi-Tenancy

### 1. Database per Tenant

Each tenant has its own database.

### 2. Schema per Tenant

Each tenant has its own schema in a shared database.

### 3. Shared Database, Shared Schema

All tenants share the same database and schema, with tenant identification.

## Implementation: Shared Database

### Tenant Context

```java
public class TenantContext {
    private static final ThreadLocal<String> currentTenant = new ThreadLocal<>();
    
    public static void setCurrentTenant(String tenant) {
        currentTenant.set(tenant);
    }
    
    public static String getCurrentTenant() {
        return currentTenant.get();
    }
    
    public static void clear() {
        currentTenant.remove();
    }
}
```

### Tenant Resolver

```java
@Component
public class TenantResolver {
    
    public String resolveTenant(HttpServletRequest request) {
        // Resolve from header
        String tenant = request.getHeader("X-Tenant-ID");
        
        // Or resolve from subdomain
        if (tenant == null) {
            String host = request.getServerName();
            tenant = host.split("\\.")[0];
        }
        
        // Or resolve from path
        if (tenant == null) {
            String path = request.getRequestURI();
            if (path.startsWith("/tenant/")) {
                tenant = path.split("/")[2];
            }
        }
        
        return tenant;
    }
}
```

### Tenant Interceptor

```java
@Component
public class TenantInterceptor implements HandlerInterceptor {
    
    private final TenantResolver tenantResolver;
    
    public TenantInterceptor(TenantResolver tenantResolver) {
        this.tenantResolver = tenantResolver;
    }
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                           Object handler) throws Exception {
        String tenant = tenantResolver.resolveTenant(request);
        
        if (tenant == null) {
            response.setStatus(HttpStatus.BAD_REQUEST.value());
            return false;
        }
        
        TenantContext.setCurrentTenant(tenant);
        return true;
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
                               Object handler, Exception ex) throws Exception {
        TenantContext.clear();
    }
}
```

### Register Interceptor

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    private final TenantInterceptor tenantInterceptor;
    
    public WebConfig(TenantInterceptor tenantInterceptor) {
        this.tenantInterceptor = tenantInterceptor;
    }
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(tenantInterceptor);
    }
}
```

## JPA Multi-Tenancy

### Multi-Tenant Configuration

```java
@Configuration
public class MultiTenantConfig {
    
    @Bean
    public MultiTenantConnectionProvider multiTenantConnectionProvider() {
        return new CurrentTenantConnectionProvider();
    }
    
    @Bean
    public CurrentTenantIdentifierResolver currentTenantIdentifierResolver() {
        return new CurrentTenantIdentifierResolverImpl();
    }
}
```

### Entity with Tenant

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "tenant_id")
    private String tenantId;
    
    private String name;
    private String email;
    // Getters and setters
}
```

### Repository with Tenant Filter

```java
@Repository
public class UserRepository {
    
    @Autowired
    private EntityManager entityManager;
    
    public List<User> findAll() {
        String tenantId = TenantContext.getCurrentTenant();
        CriteriaBuilder cb = entityManager.getCriteriaBuilder();
        CriteriaQuery<User> query = cb.createQuery(User.class);
        Root<User> root = query.from(User.class);
        query.where(cb.equal(root.get("tenantId"), tenantId));
        return entityManager.createQuery(query).getResultList();
    }
}
```

## Security

### Tenant Security

```java
@Component
public class TenantSecurityFilter extends OncePerRequestFilter {
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                    HttpServletResponse response, 
                                    FilterChain filterChain) throws ServletException, IOException {
        String tenant = TenantContext.getCurrentTenant();
        
        if (tenant == null) {
            response.setStatus(HttpStatus.FORBIDDEN.value());
            return;
        }
        
        // Verify tenant access
        if (!hasAccess(tenant, request)) {
            response.setStatus(HttpStatus.FORBIDDEN.value());
            return;
        }
        
        filterChain.doFilter(request, response);
    }
    
    private boolean hasAccess(String tenant, HttpServletRequest request) {
        // Security check logic
        return true;
    }
}
```

## Best Practices

1. **Isolate Data**: Ensure proper data isolation
2. **Secure Access**: Implement proper security
3. **Performance**: Optimize for multi-tenant queries
4. **Monitoring**: Monitor per-tenant performance
5. **Backup**: Implement tenant-specific backups

## Summary

Spring Boot Multi-Tenancy:
- Support multiple tenants
- Data isolation
- Flexible tenant resolution
- Essential for SaaS applications

