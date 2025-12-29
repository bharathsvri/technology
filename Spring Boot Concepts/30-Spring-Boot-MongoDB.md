# Spring Boot MongoDB

## What is MongoDB?

MongoDB is a NoSQL document database that stores data in flexible, JSON-like documents. It's ideal for applications with rapidly changing schemas.

## Setting Up MongoDB

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

### Configuration

```properties
# MongoDB Configuration
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.database=mydb
spring.data.mongodb.username=admin
spring.data.mongodb.password=password
spring.data.mongodb.authentication-database=admin

# Or use connection string
spring.data.mongodb.uri=mongodb://admin:password@localhost:27017/mydb?authSource=admin
```

## Document Entity

### Basic Document

```java
@Document(collection = "users")
public class User {
    @Id
    private String id;
    private String name;
    private String email;
    private Integer age;
    private Address address;
    
    // Constructors, getters, setters
}

@Embedded
public class Address {
    private String street;
    private String city;
    private String zipCode;
    // Constructors, getters, setters
}
```

## MongoDB Repository

### Repository Interface

```java
public interface UserRepository extends MongoRepository<User, String> {
    List<User> findByName(String name);
    List<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
    List<User> findByNameAndEmail(String name, String email);
}
```

### Custom Queries

```java
public interface UserRepository extends MongoRepository<User, String> {
    
    @Query("{ 'name' : ?0 }")
    List<User> findUsersByName(String name);
    
    @Query("{ 'age' : { $gte: ?0, $lte: ?1 } }")
    List<User> findUsersByAgeRange(int minAge, int maxAge);
    
    @Query(value = "{ 'name' : ?0 }", fields = "{ 'name' : 1, 'email' : 1 }")
    List<User> findUsersByNameProjection(String name);
}
```

## Service Layer

### Basic Service

```java
@Service
public class UserService {
    
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    public User getUserById(String id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }
    
    public User createUser(User user) {
        return userRepository.save(user);
    }
    
    public User updateUser(String id, User userDetails) {
        User user = getUserById(id);
        user.setName(userDetails.getName());
        user.setEmail(userDetails.getEmail());
        return userRepository.save(user);
    }
    
    public void deleteUser(String id) {
        userRepository.deleteById(id);
    }
}
```

## MongoDB Template

### Using MongoTemplate

```java
@Service
public class UserService {
    
    private final MongoTemplate mongoTemplate;
    
    public UserService(MongoTemplate mongoTemplate) {
        this.mongoTemplate = mongoTemplate;
    }
    
    public List<User> findUsersByCriteria(String name, Integer minAge) {
        Query query = new Query();
        if (name != null) {
            query.addCriteria(Criteria.where("name").is(name));
        }
        if (minAge != null) {
            query.addCriteria(Criteria.where("age").gte(minAge));
        }
        return mongoTemplate.find(query, User.class);
    }
    
    public void updateUserField(String id, String field, Object value) {
        Query query = new Query(Criteria.where("id").is(id));
        Update update = new Update().set(field, value);
        mongoTemplate.updateFirst(query, update, User.class);
    }
}
```

## Aggregation

### Using Aggregation

```java
@Service
public class UserService {
    
    private final MongoTemplate mongoTemplate;
    
    public List<AgeGroup> getUsersByAgeGroup() {
        Aggregation aggregation = Aggregation.newAggregation(
            Aggregation.group("age").count().as("count"),
            Aggregation.sort(Sort.Direction.ASC, "age")
        );
        
        AggregationResults<AgeGroup> results = mongoTemplate.aggregate(
            aggregation, "users", AgeGroup.class);
        
        return results.getMappedResults();
    }
}
```

## Transactions

### Enable Transactions

```java
@Configuration
@EnableMongoRepositories
public class MongoConfig {
    
    @Bean
    public MongoTransactionManager transactionManager(MongoDatabaseFactory dbFactory) {
        return new MongoTransactionManager(dbFactory);
    }
}
```

### Using Transactions

```java
@Service
@Transactional
public class UserService {
    
    @Transactional
    public void transferUser(String fromId, String toId) {
        User fromUser = getUserById(fromId);
        User toUser = getUserById(toId);
        // Transaction logic
    }
}
```

## Best Practices

1. **Use Indexes**: Create indexes for frequently queried fields
2. **Design Documents**: Design documents for your use case
3. **Use Projections**: Use projections to limit returned fields
4. **Handle Connections**: Configure connection pooling properly
5. **Monitor Performance**: Monitor query performance

## Summary

Spring Boot MongoDB:
- NoSQL document database
- Flexible schema
- Easy Spring Data integration
- Essential for document-based applications

