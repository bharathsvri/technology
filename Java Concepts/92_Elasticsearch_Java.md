# Elasticsearch with Java

## What is Elasticsearch?
Elasticsearch is a distributed search and analytics engine.

## Elasticsearch Java Client
```java
import org.elasticsearch.client.*;
import org.elasticsearch.action.index.*;
import org.elasticsearch.action.search.*;
import org.elasticsearch.index.query.QueryBuilders;

public class ElasticsearchExample {
    public static void main(String[] args) throws Exception {
        RestClient restClient = RestClient.builder(
            new HttpHost("localhost", 9200, "http")).build();
        
        RestHighLevelClient client = new RestHighLevelClient(restClient);
        
        // Index document
        IndexRequest request = new IndexRequest("users");
        Map<String, Object> jsonMap = new HashMap<>();
        jsonMap.put("name", "John");
        jsonMap.put("age", 25);
        request.source(jsonMap);
        IndexResponse response = client.index(request, RequestOptions.DEFAULT);
        
        // Search
        SearchRequest searchRequest = new SearchRequest("users");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        sourceBuilder.query(QueryBuilders.matchQuery("name", "John"));
        searchRequest.source(sourceBuilder);
        SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
        
        client.close();
    }
}
```

## Spring Data Elasticsearch
```java
import org.springframework.data.elasticsearch.annotations.*;
import org.springframework.data.elasticsearch.repository.*;

@Document(indexName = "users")
public class User {
    @Id
    private String id;
    
    @Field(type = FieldType.Text)
    private String name;
    
    @Field(type = FieldType.Integer)
    private Integer age;
    
    // Getters and setters
}

public interface UserRepository extends ElasticsearchRepository<User, String> {
    List<User> findByName(String name);
    List<User> findByAgeBetween(int min, int max);
}

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public User indexUser(User user) {
        return userRepository.save(user);
    }
    
    public List<User> searchUsers(String query) {
        return userRepository.findByName(query);
    }
}
```

## Best Practices
1. Design index mappings carefully
2. Use appropriate analyzers
3. Implement proper error handling
4. Use bulk operations
5. Monitor cluster health
6. Use aliases
7. Implement proper security
8. Optimize queries

