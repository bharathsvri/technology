# Spring Boot Data JPA

## What is Spring Data JPA?

Spring Data JPA is part of the larger Spring Data family. It provides an abstraction layer over JPA (Java Persistence API) and makes it easy to implement JPA-based repositories with minimal boilerplate code.

## Setting Up Spring Data JPA

### Add Dependencies

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

### Configuration

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.jpa.properties.hibernate.format_sql=true
```

## Entity Classes

### Basic Entity

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    // Constructors, getters, setters
}
```

### Entity Annotations

#### @Entity
Marks class as JPA entity:

```java
@Entity
public class User {
    // ...
}
```

#### @Table
Specifies table name:

```java
@Table(name = "users", schema = "public")
public class User {
    // ...
}
```

#### @Id
Marks primary key:

```java
@Id
private Long id;
```

#### @GeneratedValue
Defines generation strategy:

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

Generation strategies:
- `AUTO`: Let persistence provider choose
- `IDENTITY`: Database identity column
- `SEQUENCE`: Database sequence
- `TABLE`: Table-based generator

#### @Column
Customizes column mapping:

```java
@Column(name = "user_name", nullable = false, length = 50, unique = true)
private String name;
```

#### @Transient
Excludes field from persistence:

```java
@Transient
private String temporaryField;
```

## Relationships

### One-to-One

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id")
    private Address address;
}

@Entity
public class Address {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToOne(mappedBy = "address")
    private User user;
}
```

### One-to-Many

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
    private List<Order> orders;
}

@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
}
```

### Many-to-Many

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToMany
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;
}

@Entity
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToMany(mappedBy = "roles")
    private Set<User> users;
}
```

## Repository Interfaces

### JpaRepository

```java
public interface UserRepository extends JpaRepository<User, Long> {
    // Inherits basic CRUD operations
}
```

### CrudRepository Methods

- `save(entity)` - Save entity
- `findById(id)` - Find by ID
- `findAll()` - Find all
- `deleteById(id)` - Delete by ID
- `count()` - Count entities
- `existsById(id)` - Check existence

### Custom Query Methods

#### By Method Name

```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
    List<User> findByEmail(String email);
    List<User> findByNameAndEmail(String name, String email);
    List<User> findByNameOrEmail(String name, String email);
    List<User> findByAgeGreaterThan(int age);
    List<User> findByAgeBetween(int min, int max);
    List<User> findByNameContaining(String name);
    List<User> findByNameLike(String pattern);
    List<User> findByEmailIsNull();
    List<User> findByEmailIsNotNull();
    List<User> findTop10ByOrderByAgeDesc();
}
```

#### Using @Query

```java
@Query("SELECT u FROM User u WHERE u.email = ?1")
User findByEmail(String email);

@Query("SELECT u FROM User u WHERE u.age > :age")
List<User> findUsersOlderThan(@Param("age") int age);

@Query(value = "SELECT * FROM users WHERE name = ?1", nativeQuery = true)
List<User> findByNameNative(String name);
```

#### Using @Modifying

```java
@Modifying
@Query("UPDATE User u SET u.name = :name WHERE u.id = :id")
void updateUserName(@Param("id") Long id, @Param("name") String name);

@Modifying
@Query("DELETE FROM User u WHERE u.email = :email")
void deleteByEmail(@Param("email") String email);
```

## Service Layer

### Basic Service

```java
@Service
@Transactional
public class UserService {
    
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    public User getUserById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }
    
    public User createUser(User user) {
        return userRepository.save(user);
    }
    
    public User updateUser(Long id, User userDetails) {
        User user = getUserById(id);
        user.setName(userDetails.getName());
        user.setEmail(userDetails.getEmail());
        return userRepository.save(user);
    }
    
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

## Pagination and Sorting

### Pagination

```java
public interface UserRepository extends JpaRepository<User, Long> {
    Page<User> findByName(String name, Pageable pageable);
}

// Usage
Pageable pageable = PageRequest.of(0, 10);
Page<User> users = userRepository.findByName("John", pageable);
```

### Sorting

```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name, Sort sort);
}

// Usage
Sort sort = Sort.by("name").ascending();
List<User> users = userRepository.findByName("John", sort);
```

## JPA Specifications

### Using Specifications

```java
public interface UserRepository extends JpaRepository<User, Long>, JpaSpecificationExecutor<User> {
}

// Usage
Specification<User> spec = (root, query, cb) -> {
    return cb.equal(root.get("name"), "John");
};
List<User> users = userRepository.findAll(spec);
```

## Entity Lifecycle Callbacks

### @PrePersist and @PostPersist

```java
@Entity
public class User {
    @PrePersist
    public void prePersist() {
        this.createdAt = LocalDateTime.now();
    }
    
    @PostPersist
    public void postPersist() {
        System.out.println("User persisted: " + this.id);
    }
}
```

### @PreUpdate and @PostUpdate

```java
@PreUpdate
public void preUpdate() {
    this.updatedAt = LocalDateTime.now();
}

@PostUpdate
public void postUpdate() {
    System.out.println("User updated: " + this.id);
}
```

### @PreRemove and @PostRemove

```java
@PreRemove
public void preRemove() {
    System.out.println("Removing user: " + this.id);
}

@PostRemove
public void postRemove() {
    System.out.println("User removed: " + this.id);
}
```

## Hibernate DDL Auto

### Options

- `none`: No schema management
- `validate`: Validate schema only
- `update`: Update schema if needed
- `create`: Drop and create schema
- `create-drop`: Drop schema on shutdown

```properties
spring.jpa.hibernate.ddl-auto=update
```

## Database Initialization

### Using schema.sql and data.sql

```sql
-- schema.sql
CREATE TABLE IF NOT EXISTS users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);
```

```sql
-- data.sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```

```properties
spring.sql.init.mode=always
```

## Multiple Data Sources

### Configuration

```java
@Configuration
@EnableJpaRepositories(
    basePackages = "com.example.repository.primary",
    entityManagerFactoryRef = "primaryEntityManagerFactory",
    transactionManagerRef = "primaryTransactionManager"
)
public class PrimaryDataSourceConfig {
    
    @Primary
    @Bean
    @ConfigurationProperties("spring.datasource.primary")
    public DataSource primaryDataSource() {
        return DataSourceBuilder.create().build();
    }
    
    @Primary
    @Bean
    public LocalContainerEntityManagerFactoryBean primaryEntityManagerFactory(
            EntityManagerFactoryBuilder builder) {
        return builder
            .dataSource(primaryDataSource())
            .packages("com.example.entity.primary")
            .build();
    }
    
    @Primary
    @Bean
    public PlatformTransactionManager primaryTransactionManager(
            @Qualifier("primaryEntityManagerFactory") EntityManagerFactory entityManagerFactory) {
        return new JpaTransactionManager(entityManagerFactory);
    }
}
```

## Best Practices

1. **Use @Transactional**: Use `@Transactional` in service layer
2. **Lazy Loading**: Use lazy loading for relationships
3. **Avoid N+1 Problem**: Use JOIN FETCH or Entity Graphs
4. **Use Projections**: Use DTOs for read operations
5. **Batch Operations**: Use batch inserts/updates for bulk operations
6. **Indexing**: Add indexes for frequently queried columns
7. **Connection Pooling**: Configure connection pool properly
8. **Query Optimization**: Use appropriate fetch strategies

## Summary

Spring Data JPA:
- Provides repository abstraction
- Reduces boilerplate code
- Supports custom queries
- Handles relationships
- Supports pagination and sorting
- Provides entity lifecycle callbacks
- Easy to configure and use

