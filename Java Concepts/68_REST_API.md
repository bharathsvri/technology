# REST API Development

## What is REST?
REST (Representational State Transfer) is an architectural style for designing web services.

## REST Principles
- Stateless
- Client-Server
- Cacheable
- Uniform Interface
- Layered System

## JAX-RS Basics (Jersey)

### Setup
```xml
<dependency>
    <groupId>org.glassfish.jersey.containers</groupId>
    <artifactId>jersey-container-servlet</artifactId>
    <version>3.1.3</version>
</dependency>
<dependency>
    <groupId>org.glassfish.jersey.inject</groupId>
    <artifactId>jersey-hk2</artifactId>
    <version>3.1.3</version>
</dependency>
```

### Basic REST Endpoint
```java
import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;

@Path("/users")
public class UserResource {
    
    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Response getAllUsers() {
        // Return list of users
        return Response.ok().entity(users).build();
    }
    
    @GET
    @Path("/{id}")
    @Produces(MediaType.APPLICATION_JSON)
    public Response getUser(@PathParam("id") Long id) {
        User user = findUser(id);
        if (user == null) {
            return Response.status(404).build();
        }
        return Response.ok().entity(user).build();
    }
    
    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    public Response createUser(User user) {
        User created = saveUser(user);
        return Response.status(201).entity(created).build();
    }
    
    @PUT
    @Path("/{id}")
    @Consumes(MediaType.APPLICATION_JSON)
    public Response updateUser(@PathParam("id") Long id, User user) {
        updateUser(id, user);
        return Response.noContent().build();
    }
    
    @DELETE
    @Path("/{id}")
    public Response deleteUser(@PathParam("id") Long id) {
        deleteUser(id);
        return Response.noContent().build();
    }
}
```

## HTTP Status Codes
```java
@GET
@Path("/{id}")
public Response getUser(@PathParam("id") Long id) {
    User user = findUser(id);
    if (user == null) {
        return Response.status(Response.Status.NOT_FOUND).build();
    }
    return Response.ok(user).build();
}
```

## Query Parameters
```java
@GET
@Path("/search")
public Response searchUsers(
    @QueryParam("name") String name,
    @QueryParam("age") Integer age,
    @QueryParam("page") @DefaultValue("0") int page,
    @QueryParam("size") @DefaultValue("10") int size) {
    
    // Search logic
    return Response.ok(results).build();
}
```

## Request/Response Entities
```java
@POST
@Consumes(MediaType.APPLICATION_JSON)
@Produces(MediaType.APPLICATION_JSON)
public Response createUser(User user) {
    // Process user
    return Response.status(201).entity(user).build();
}
```

## Exception Handling
```java
@Provider
public class NotFoundExceptionMapper 
        implements ExceptionMapper<NotFoundException> {
    @Override
    public Response toResponse(NotFoundException exception) {
        return Response.status(404)
            .entity("Resource not found")
            .build();
    }
}
```

## Best Practices
1. Use proper HTTP methods
2. Return appropriate status codes
3. Use consistent URL patterns
4. Version your API
5. Document with OpenAPI/Swagger
6. Handle errors gracefully
7. Use appropriate content types
8. Implement pagination for lists
9. Use HTTPS
10. Validate input

