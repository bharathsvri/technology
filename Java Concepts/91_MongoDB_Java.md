# MongoDB with Java

## What is MongoDB?
MongoDB is a NoSQL document database.

## MongoDB Java Driver
```java
import com.mongodb.client.*;
import org.bson.Document;

public class MongoDBExample {
    public static void main(String[] args) {
        MongoClient client = MongoClients.create("mongodb://localhost:27017");
        MongoDatabase database = client.getDatabase("mydb");
        MongoCollection<Document> collection = database.getCollection("users");
        
        // Insert
        Document user = new Document("name", "John")
            .append("age", 25)
            .append("email", "john@example.com");
        collection.insertOne(user);
        
        // Find
        Document found = collection.find(eq("name", "John")).first();
        
        // Update
        collection.updateOne(eq("name", "John"), 
            new Document("$set", new Document("age", 26)));
        
        // Delete
        collection.deleteOne(eq("name", "John"));
        
        client.close();
    }
}
```

## Spring Data MongoDB
```java
import org.springframework.data.mongodb.core.mapping.*;
import org.springframework.data.mongodb.repository.*;

@Document(collection = "users")
public class User {
    @Id
    private String id;
    private String name;
    private int age;
    private String email;
    
    // Getters and setters
}

public interface UserRepository extends MongoRepository<User, String> {
    List<User> findByName(String name);
    List<User> findByAgeGreaterThan(int age);
}

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public User createUser(User user) {
        return userRepository.save(user);
    }
    
    public List<User> findUsersByName(String name) {
        return userRepository.findByName(name);
    }
}
```

## Aggregation
```java
import org.springframework.data.mongodb.core.aggregation.*;

Aggregation aggregation = Aggregation.newAggregation(
    Aggregation.match(Criteria.where("age").gte(18)),
    Aggregation.group("department").avg("salary").as("avgSalary"),
    Aggregation.sort(Sort.Direction.DESC, "avgSalary")
);

List<Document> results = mongoTemplate.aggregate(
    aggregation, "users", Document.class).getMappedResults();
```

## Best Practices
1. Design schema appropriately
2. Use indexes
3. Handle connection pooling
4. Use transactions when needed
5. Implement proper error handling
6. Use aggregation for complex queries
7. Monitor performance
8. Secure database access

