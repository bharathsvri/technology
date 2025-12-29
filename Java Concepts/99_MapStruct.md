# MapStruct

## What is MapStruct?
MapStruct is a code generator that simplifies mapping between Java bean types.

## Setup
```xml
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>1.5.5.Final</version>
</dependency>
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-processor</artifactId>
    <version>1.5.5.Final</version>
    <scope>provided</scope>
</dependency>
```

## Basic Mapper
```java
import org.mapstruct.*;

@Mapper
public interface UserMapper {
    UserMapper INSTANCE = Mappers.getMapper(UserMapper.class);
    
    UserDTO toDTO(User user);
    User toEntity(UserDTO dto);
    
    List<UserDTO> toDTOList(List<User> users);
}
```

## Custom Mapping
```java
@Mapper
public interface UserMapper {
    @Mapping(source = "fullName", target = "name")
    @Mapping(source = "emailAddress", target = "email")
    @Mapping(target = "id", ignore = true)
    UserDTO toDTO(User user);
}
```

## Multiple Source Objects
```java
@Mapper
public interface UserMapper {
    @Mapping(source = "user.name", target = "name")
    @Mapping(source = "address.city", target = "city")
    UserDTO toDTO(User user, Address address);
}
```

## Custom Methods
```java
@Mapper
public interface UserMapper {
    UserDTO toDTO(User user);
    
    default String mapStatus(UserStatus status) {
        return status == UserStatus.ACTIVE ? "A" : "I";
    }
}
```

## Best Practices
1. Use MapStruct for complex mappings
2. Keep mappers focused
3. Use custom methods when needed
4. Handle null values
5. Test mappings thoroughly
6. Use appropriate mapping strategies
7. Document complex mappings
8. Consider performance

