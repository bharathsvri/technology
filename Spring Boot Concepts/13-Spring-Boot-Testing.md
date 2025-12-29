# Spring Boot Testing

## Testing in Spring Boot

Spring Boot provides comprehensive testing support with JUnit 5, Mockito, AssertJ, and other testing libraries through the `spring-boot-starter-test` dependency.

## Setting Up Testing

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

### Test Dependencies Included

- JUnit 5
- Spring Test & Spring Boot Test
- AssertJ
- Hamcrest
- Mockito
- JSONassert
- JsonPath

## Unit Testing

### Simple Unit Test

```java
class UserServiceTest {
    
    private UserService userService;
    private UserRepository userRepository;
    
    @BeforeEach
    void setUp() {
        userRepository = mock(UserRepository.class);
        userService = new UserService(userRepository);
    }
    
    @Test
    void testGetUserById() {
        // Given
        Long id = 1L;
        User user = new User("John", "john@example.com");
        when(userRepository.findById(id)).thenReturn(Optional.of(user));
        
        // When
        User result = userService.getUserById(id);
        
        // Then
        assertThat(result).isNotNull();
        assertThat(result.getName()).isEqualTo("John");
        verify(userRepository).findById(id);
    }
}
```

## Integration Testing

### @SpringBootTest

```java
@SpringBootTest
class UserServiceIntegrationTest {
    
    @Autowired
    private UserService userService;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void testCreateUser() {
        // Given
        User user = new User("John", "john@example.com");
        
        // When
        User savedUser = userService.createUser(user);
        
        // Then
        assertThat(savedUser.getId()).isNotNull();
        assertThat(savedUser.getName()).isEqualTo("John");
    }
    
    @AfterEach
    void tearDown() {
        userRepository.deleteAll();
    }
}
```

### @WebMvcTest

Tests only web layer:

```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void testGetUser() throws Exception {
        // Given
        User user = new User("John", "john@example.com");
        when(userService.getUserById(1L)).thenReturn(user);
        
        // When & Then
        mockMvc.perform(get("/api/users/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("John"))
            .andExpect(jsonPath("$.email").value("john@example.com"));
    }
    
    @Test
    void testCreateUser() throws Exception {
        // Given
        User user = new User("John", "john@example.com");
        when(userService.createUser(any(User.class))).thenReturn(user);
        
        // When & Then
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"John\",\"email\":\"john@example.com\"}"))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.name").value("John"));
    }
}
```

### @DataJpaTest

Tests only data layer:

```java
@DataJpaTest
class UserRepositoryTest {
    
    @Autowired
    private TestEntityManager entityManager;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void testFindByName() {
        // Given
        User user = new User("John", "john@example.com");
        entityManager.persist(user);
        entityManager.flush();
        
        // When
        Optional<User> found = userRepository.findByName("John");
        
        // Then
        assertThat(found).isPresent();
        assertThat(found.get().getName()).isEqualTo("John");
    }
}
```

### @JsonTest

Tests JSON serialization/deserialization:

```java
@JsonTest
class UserJsonTest {
    
    @Autowired
    private JacksonTester<User> json;
    
    @Test
    void testSerialize() throws Exception {
        User user = new User("John", "john@example.com");
        
        assertThat(json.write(user))
            .extractingJsonPathStringValue("$.name")
            .isEqualTo("John");
    }
    
    @Test
    void testDeserialize() throws Exception {
        String jsonContent = "{\"name\":\"John\",\"email\":\"john@example.com\"}";
        
        assertThat(json.parse(jsonContent))
            .isEqualTo(new User("John", "john@example.com"));
    }
}
```

## MockMvc Testing

### Testing REST Controllers

```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerIntegrationTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void testGetAllUsers() throws Exception {
        // Given
        List<User> users = Arrays.asList(
            new User("John", "john@example.com"),
            new User("Jane", "jane@example.com")
        );
        when(userService.getAllUsers()).thenReturn(users);
        
        // When & Then
        mockMvc.perform(get("/api/users"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$", hasSize(2)))
            .andExpect(jsonPath("$[0].name").value("John"));
    }
    
    @Test
    void testDeleteUser() throws Exception {
        // When & Then
        mockMvc.perform(delete("/api/users/1"))
            .andExpect(status().isNoContent());
        
        verify(userService).deleteUser(1L);
    }
}
```

## Test Slices

### Available Test Slices

- `@WebMvcTest` - Web layer
- `@DataJpaTest` - Data layer
- `@JsonTest` - JSON serialization
- `@DataMongoTest` - MongoDB
- `@RestClientTest` - REST clients
- `@WebFluxTest` - WebFlux

## Test Configuration

### @TestConfiguration

```java
@TestConfiguration
public class TestConfig {
    
    @Bean
    @Primary
    public UserService testUserService() {
        return mock(UserService.class);
    }
}
```

### application-test.properties

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
```

```java
@SpringBootTest
@TestPropertySource(locations = "classpath:application-test.properties")
class UserServiceTest {
    // Uses test properties
}
```

## Database Testing

### Using H2 In-Memory Database

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.jpa.hibernate.ddl-auto=create-drop
```

### Using @Sql

```java
@SpringBootTest
@Sql(scripts = "/test-data.sql")
class UserServiceTest {
    // Test data loaded from SQL file
}
```

### Using @Transactional

```java
@SpringBootTest
@Transactional
class UserServiceTest {
    
    @Test
    void testCreateUser() {
        // Transaction rolls back after test
    }
}
```

## Mocking

### @MockBean

Mocks Spring beans:

```java
@SpringBootTest
class UserServiceTest {
    
    @MockBean
    private UserRepository userRepository;
    
    @Test
    void testGetUser() {
        when(userRepository.findById(1L))
            .thenReturn(Optional.of(new User("John", "john@example.com")));
        // Test implementation
    }
}
```

### @SpyBean

Spies on Spring beans:

```java
@SpringBootTest
class UserServiceTest {
    
    @SpyBean
    private UserService userService;
    
    @Test
    void testGetUser() {
        doReturn(new User("John", "john@example.com"))
            .when(userService).getUserById(1L);
        // Test implementation
    }
}
```

## Assertions

### AssertJ Assertions

```java
@Test
void testAssertions() {
    User user = new User("John", "john@example.com");
    
    assertThat(user)
        .isNotNull()
        .extracting(User::getName, User::getEmail)
        .containsExactly("John", "john@example.com");
    
    List<User> users = Arrays.asList(user);
    assertThat(users)
        .hasSize(1)
        .contains(user);
}
```

### JUnit Assertions

```java
@Test
void testJUnitAssertions() {
    User user = new User("John", "john@example.com");
    
    assertNotNull(user);
    assertEquals("John", user.getName());
    assertTrue(user.getEmail().contains("@"));
}
```

## Testing REST APIs

### Complete REST API Test

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
class UserControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void testCreateUser() throws Exception {
        String userJson = """
            {
                "name": "John",
                "email": "john@example.com",
                "age": 30
            }
            """;
        
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(userJson))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.id").exists())
            .andExpect(jsonPath("$.name").value("John"));
    }
    
    @Test
    void testGetUserNotFound() throws Exception {
        mockMvc.perform(get("/api/users/999"))
            .andExpect(status().isNotFound());
    }
}
```

## Testing with TestContainers

### Add Dependency

```xml
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>mysql</artifactId>
    <scope>test</scope>
</dependency>
```

### Using TestContainers

```java
@SpringBootTest
@Testcontainers
class UserServiceIntegrationTest {
    
    @Container
    static MySQLContainer<?> mysql = new MySQLContainer<>("mysql:8.0")
            .withDatabaseName("testdb")
            .withUsername("test")
            .withPassword("test");
    
    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", mysql::getJdbcUrl);
        registry.add("spring.datasource.username", mysql::getUsername);
        registry.add("spring.datasource.password", mysql::getPassword);
    }
    
    @Test
    void testWithRealDatabase() {
        // Test with real MySQL database
    }
}
```

## Best Practices

1. **Use Appropriate Test Slices**: Use specific test slices when possible
2. **Keep Tests Independent**: Tests should not depend on each other
3. **Use Descriptive Test Names**: Name tests clearly
4. **Follow AAA Pattern**: Arrange, Act, Assert
5. **Mock External Dependencies**: Mock external services
6. **Use In-Memory Database**: Use H2 for integration tests
7. **Clean Up After Tests**: Clean up test data
8. **Test Edge Cases**: Test boundary conditions and errors

## Summary

Spring Boot Testing:
- Provides comprehensive testing support
- Includes multiple test slices
- Supports unit and integration testing
- Easy to mock and spy beans
- Supports database testing
- Essential for reliable applications

