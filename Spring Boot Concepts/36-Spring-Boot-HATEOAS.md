# Spring Boot HATEOAS

## What is HATEOAS?

HATEOAS (Hypermedia as the Engine of Application State) is a constraint of REST that allows clients to interact with RESTful services by following hypermedia links rather than hardcoding URLs.

## Setting Up HATEOAS

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```

## Entity Representation

### Using EntityModel

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    private final UserService userService;
    private final EntityLinks entityLinks;
    
    @GetMapping("/{id}")
    public EntityModel<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        EntityModel<User> userModel = EntityModel.of(user);
        userModel.add(linkTo(methodOn(UserController.class).getUser(id)).withSelfRel());
        userModel.add(linkTo(methodOn(UserController.class).getAllUsers()).withRel("users"));
        return userModel;
    }
}
```

### Using CollectionModel

```java
@GetMapping
public CollectionModel<EntityModel<User>> getAllUsers() {
    List<EntityModel<User>> users = userService.getAllUsers().stream()
        .map(user -> EntityModel.of(user,
            linkTo(methodOn(UserController.class).getUser(user.getId())).withSelfRel(),
            linkTo(methodOn(UserController.class).getAllUsers()).withRel("users")))
        .collect(Collectors.toList());
    
    return CollectionModel.of(users,
        linkTo(methodOn(UserController.class).getAllUsers()).withSelfRel());
}
```

## PagedModel

### Pagination with HATEOAS

```java
@GetMapping
public PagedModel<EntityModel<User>> getAllUsers(Pageable pageable) {
    Page<User> users = userService.getAllUsers(pageable);
    
    List<EntityModel<User>> userModels = users.getContent().stream()
        .map(user -> EntityModel.of(user,
            linkTo(methodOn(UserController.class).getUser(user.getId())).withSelfRel()))
        .collect(Collectors.toList());
    
    PagedModel.PageMetadata pageMetadata = new PagedModel.PageMetadata(
        users.getSize(), users.getNumber(), users.getTotalElements(), users.getTotalPages());
    
    return PagedModel.of(userModels, pageMetadata,
        linkTo(methodOn(UserController.class).getAllUsers(pageable)).withSelfRel());
}
```

## Custom Links

### Adding Custom Links

```java
@GetMapping("/{id}")
public EntityModel<User> getUser(@PathVariable Long id) {
    User user = userService.getUserById(id);
    EntityModel<User> userModel = EntityModel.of(user);
    
    // Self link
    userModel.add(linkTo(methodOn(UserController.class).getUser(id)).withSelfRel());
    
    // Related resources
    userModel.add(linkTo(methodOn(OrderController.class).getUserOrders(id)).withRel("orders"));
    userModel.add(linkTo(methodOn(AddressController.class).getUserAddress(id)).withRel("address"));
    
    // Actions
    if (user.isActive()) {
        userModel.add(linkTo(methodOn(UserController.class).deactivateUser(id)).withRel("deactivate"));
    } else {
        userModel.add(linkTo(methodOn(UserController.class).activateUser(id)).withRel("activate"));
    }
    
    return userModel;
}
```

## Representation Model Assembler

### Custom Assembler

```java
@Component
public class UserModelAssembler implements RepresentationModelAssembler<User, EntityModel<User>> {
    
    @Override
    public EntityModel<User> toModel(User user) {
        return EntityModel.of(user,
            linkTo(methodOn(UserController.class).getUser(user.getId())).withSelfRel(),
            linkTo(methodOn(UserController.class).getAllUsers()).withRel("users"));
    }
}
```

### Using Assembler

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    private final UserModelAssembler assembler;
    
    @GetMapping("/{id}")
    public EntityModel<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return assembler.toModel(user);
    }
}
```

## Complete Example

### Controller

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    private final UserService userService;
    private final UserModelAssembler assembler;
    
    @GetMapping
    public CollectionModel<EntityModel<User>> getAllUsers() {
        List<EntityModel<User>> users = userService.getAllUsers().stream()
            .map(assembler::toModel)
            .collect(Collectors.toList());
        
        return CollectionModel.of(users,
            linkTo(methodOn(UserController.class).getAllUsers()).withSelfRel());
    }
    
    @GetMapping("/{id}")
    public EntityModel<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return assembler.toModel(user);
    }
    
    @PostMapping
    public ResponseEntity<EntityModel<User>> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        EntityModel<User> userModel = assembler.toModel(createdUser);
        
        return ResponseEntity
            .created(userModel.getRequiredLink(IanaLinkRelations.SELF).toUri())
            .body(userModel);
    }
}
```

## Best Practices

1. **Include Self Links**: Always include self links
2. **Use Meaningful Rel Names**: Use descriptive relation names
3. **Conditional Links**: Add links based on state
4. **Use Assemblers**: Use assemblers for consistency
5. **Document Links**: Document available links

## Summary

Spring Boot HATEOAS:
- Enables hypermedia-driven REST APIs
- Makes APIs more discoverable
- Reduces client-server coupling
- Essential for RESTful API design

