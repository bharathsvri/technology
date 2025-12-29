# Jackson Advanced

## Custom Serialization
```java
import com.fasterxml.jackson.core.*;
import com.fasterxml.jackson.databind.*;

public class CustomSerializer extends JsonSerializer<User> {
    @Override
    public void serialize(User user, JsonGenerator gen, 
                          SerializerProvider serializers) throws IOException {
        gen.writeStartObject();
        gen.writeStringField("name", user.getName());
        gen.writeNumberField("age", user.getAge());
        gen.writeEndObject();
    }
}

@JsonSerialize(using = CustomSerializer.class)
public class User {
    private String name;
    private int age;
}
```

## Custom Deserialization
```java
import com.fasterxml.jackson.core.*;
import com.fasterxml.jackson.databind.*;

public class CustomDeserializer extends JsonDeserializer<User> {
    @Override
    public User deserialize(JsonParser p, DeserializationContext ctxt) 
            throws IOException {
        JsonNode node = p.getCodec().readTree(p);
        User user = new User();
        user.setName(node.get("name").asText());
        user.setAge(node.get("age").asInt());
        return user;
    }
}

@JsonDeserialize(using = CustomDeserializer.class)
public class User {
    // ...
}
```

## Annotations
```java
import com.fasterxml.jackson.annotation.*;

@JsonIgnoreProperties({"password", "secret"})
@JsonInclude(JsonInclude.Include.NON_NULL)
public class User {
    @JsonProperty("user_name")
    private String name;
    
    @JsonIgnore
    private String password;
    
    @JsonFormat(pattern = "yyyy-MM-dd")
    private Date birthDate;
    
    @JsonAlias({"email", "emailAddress"})
    private String email;
}
```

## ObjectMapper Configuration
```java
import com.fasterxml.jackson.databind.*;

ObjectMapper mapper = new ObjectMapper();
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
mapper.setPropertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE);
```

## Streaming API
```java
import com.fasterxml.jackson.core.*;

JsonFactory factory = new JsonFactory();
JsonGenerator generator = factory.createGenerator(new File("output.json"));
generator.writeStartObject();
generator.writeStringField("name", "John");
generator.writeNumberField("age", 25);
generator.writeEndObject();
generator.close();
```

## Best Practices
1. Use annotations appropriately
2. Handle null values
3. Configure ObjectMapper properly
4. Use streaming for large JSON
5. Customize serialization when needed
6. Handle date formats
7. Use appropriate naming strategies
8. Test with different JSON structures

