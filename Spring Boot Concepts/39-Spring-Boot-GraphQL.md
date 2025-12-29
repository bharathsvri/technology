# Spring Boot GraphQL

## What is GraphQL?

GraphQL is a query language for APIs and a runtime for executing those queries. It allows clients to request exactly the data they need.

## Setting Up GraphQL

### Add Dependency

```xml
<dependency>
    <groupId>com.graphql-java</groupId>
    <artifactId>graphql-spring-boot-starter</artifactId>
    <version>5.0.2</version>
</dependency>
<dependency>
    <groupId>com.graphql-java</groupId>
    <artifactId>graphql-java-tools</artifactId>
    <version>5.2.4</version>
</dependency>
```

### Configuration

```properties
graphql.servlet.enabled=true
graphql.servlet.mapping=/graphql
graphql.servlet.corsEnabled=true
```

## Schema Definition

### GraphQL Schema

```graphql
# schema.graphqls
type Query {
    user(id: ID!): User
    users: [User]
}

type User {
    id: ID!
    name: String!
    email: String!
    age: Int
    orders: [Order]
}

type Order {
    id: ID!
    total: Float!
    items: [OrderItem]
}

type OrderItem {
    id: ID!
    product: String!
    quantity: Int!
    price: Float!
}

type Mutation {
    createUser(name: String!, email: String!, age: Int): User
    updateUser(id: ID!, name: String, email: String): User
    deleteUser(id: ID!): Boolean
}
```

## Query Resolver

### Implementing Resolvers

```java
@Component
public class UserQueryResolver implements GraphQLQueryResolver {
    
    private final UserService userService;
    
    public UserQueryResolver(UserService userService) {
        this.userService = userService;
    }
    
    public User user(Long id) {
        return userService.getUserById(id);
    }
    
    public List<User> users() {
        return userService.getAllUsers();
    }
}
```

## Mutation Resolver

### Implementing Mutations

```java
@Component
public class UserMutationResolver implements GraphQLMutationResolver {
    
    private final UserService userService;
    
    public UserMutationResolver(UserService userService) {
        this.userService = userService;
    }
    
    public User createUser(String name, String email, Integer age) {
        User user = new User();
        user.setName(name);
        user.setEmail(email);
        user.setAge(age);
        return userService.createUser(user);
    }
    
    public User updateUser(Long id, String name, String email) {
        return userService.updateUser(id, name, email);
    }
    
    public Boolean deleteUser(Long id) {
        userService.deleteUser(id);
        return true;
    }
}
```

## Field Resolver

### Resolving Nested Fields

```java
@Component
public class UserResolver implements GraphQLResolver<User> {
    
    private final OrderService orderService;
    
    public UserResolver(OrderService orderService) {
        this.orderService = orderService;
    }
    
    public List<Order> orders(User user) {
        return orderService.getOrdersByUserId(user.getId());
    }
}
```

## Input Types

### Using Input Types

```graphql
input UserInput {
    name: String!
    email: String!
    age: Int
}

type Mutation {
    createUser(input: UserInput!): User
}
```

### Java Implementation

```java
public class UserInput {
    private String name;
    private String email;
    private Integer age;
    // Getters and setters
}

@Component
public class UserMutationResolver implements GraphQLMutationResolver {
    
    public User createUser(UserInput input) {
        User user = new User();
        user.setName(input.getName());
        user.setEmail(input.getEmail());
        user.setAge(input.getAge());
        return userService.createUser(user);
    }
}
```

## Error Handling

### Custom Exception Handler

```java
@Component
public class GraphQLExceptionHandler implements GraphQLErrorHandler {
    
    @Override
    public List<GraphQLError> processErrors(List<GraphQLError> errors) {
        return errors.stream()
            .map(this::getGraphQLError)
            .collect(Collectors.toList());
    }
    
    private GraphQLError getGraphQLError(GraphQLError error) {
        if (error instanceof ExceptionWhileDataFetching) {
            ExceptionWhileDataFetching exceptionError = (ExceptionWhileDataFetching) error;
            if (exceptionError.getException() instanceof ResourceNotFoundException) {
                return new SimpleGraphQLError(exceptionError.getException().getMessage());
            }
        }
        return error;
    }
}
```

## Testing GraphQL

### GraphQL Test

```java
@SpringBootTest
@AutoConfigureMockMvc
class GraphQLTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void testGetUser() throws Exception {
        String query = """
            query {
                user(id: 1) {
                    id
                    name
                    email
                }
            }
            """;
        
        mockMvc.perform(post("/graphql")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"query\":\"" + query + "\"}"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.data.user.name").exists());
    }
}
```

## Best Practices

1. **Design Schema First**: Design GraphQL schema before implementation
2. **Use Input Types**: Use input types for mutations
3. **Handle Errors**: Implement proper error handling
4. **Optimize Queries**: Use DataLoader for N+1 problem
5. **Document Schema**: Document your GraphQL schema

## Summary

Spring Boot GraphQL:
- Flexible query language
- Client-driven data fetching
- Single endpoint for all operations
- Essential for modern APIs

