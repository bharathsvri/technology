# GraphQL with Java

## What is GraphQL?
GraphQL is a query language and runtime for APIs that allows clients to request exactly the data they need.

## GraphQL Java Setup
```xml
<dependency>
    <groupId>com.graphql-java</groupId>
    <artifactId>graphql-java</artifactId>
    <version>20.4</version>
</dependency>
```

## Schema Definition
```java
import graphql.schema.*;

public class GraphQLSchemaBuilder {
    public static GraphQLSchema buildSchema() {
        return GraphQLSchema.newSchema()
            .query(queryType())
            .build();
    }
    
    private static GraphQLObjectType queryType() {
        return GraphQLObjectType.newObject()
            .name("Query")
            .field(GraphQLFieldDefinition.newFieldDefinition()
                .name("user")
                .type(userType())
                .argument(GraphQLArgument.newArgument()
                    .name("id")
                    .type(Scalars.GraphQLInt))
                .dataFetcher(environment -> {
                    int id = environment.getArgument("id");
                    return getUserById(id);
                }))
            .build();
    }
    
    private static GraphQLObjectType userType() {
        return GraphQLObjectType.newObject()
            .name("User")
            .field(GraphQLFieldDefinition.newFieldDefinition()
                .name("id")
                .type(Scalars.GraphQLInt))
            .field(GraphQLFieldDefinition.newFieldDefinition()
                .name("name")
                .type(Scalars.GraphQLString))
            .build();
    }
}
```

## GraphQL Query
```graphql
query {
  user(id: 1) {
    id
    name
    email
  }
}
```

## Spring GraphQL
```java
import org.springframework.graphql.data.method.annotation.*;

@Controller
public class UserController {
    
    @QueryMapping
    public User user(@Argument Integer id) {
        return userService.findById(id);
    }
    
    @MutationMapping
    public User createUser(@Argument String name, @Argument String email) {
        return userService.create(name, email);
    }
}
```

## Best Practices
1. Design schema carefully
2. Use DataLoader for N+1 problem
3. Implement proper error handling
4. Use subscriptions for real-time
5. Validate queries
6. Implement rate limiting
7. Use field-level security
8. Document schema

