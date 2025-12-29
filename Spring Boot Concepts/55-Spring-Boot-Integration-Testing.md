# Spring Boot Integration Testing

## Integration Testing Strategies

Integration testing verifies that different parts of your application work together correctly. Spring Boot provides excellent support for integration testing.

## @SpringBootTest

### Full Integration Test

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserControllerIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void testCreateUser() {
        User user = new User("John", "john@example.com");
        ResponseEntity<User> response = restTemplate.postForEntity(
            "/api/users", user, User.class);
        
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        assertThat(response.getBody().getName()).isEqualTo("John");
    }
    
    @AfterEach
    void tearDown() {
        userRepository.deleteAll();
    }
}
```

## @WebMvcTest

### Web Layer Test

```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void testGetUser() throws Exception {
        User user = new User("John", "john@example.com");
        when(userService.getUserById(1L)).thenReturn(user);
        
        mockMvc.perform(get("/api/users/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("John"));
    }
}
```

## @DataJpaTest

### Data Layer Test

```java
@DataJpaTest
class UserRepositoryTest {
    
    @Autowired
    private TestEntityManager entityManager;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void testFindByName() {
        User user = new User("John", "john@example.com");
        entityManager.persist(user);
        entityManager.flush();
        
        Optional<User> found = userRepository.findByName("John");
        
        assertThat(found).isPresent();
        assertThat(found.get().getName()).isEqualTo("John");
    }
}
```

## TestContainers

### Database Integration Test

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
    
    @Autowired
    private UserService userService;
    
    @Test
    void testCreateUser() {
        User user = new User("John", "john@example.com");
        User savedUser = userService.createUser(user);
        
        assertThat(savedUser.getId()).isNotNull();
    }
}
```

## REST Assured

### API Testing

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserApiTest {
    
    @LocalServerPort
    private int port;
    
    @BeforeEach
    void setUp() {
        RestAssured.port = port;
    }
    
    @Test
    void testGetUser() {
        given()
            .when()
            .get("/api/users/1")
            .then()
            .statusCode(200)
            .body("name", equalTo("John"));
    }
}
```

## Best Practices

1. **Test Real Scenarios**: Test real integration scenarios
2. **Isolate Tests**: Keep tests independent
3. **Use TestContainers**: Use TestContainers for database tests
4. **Clean Up**: Clean up test data
5. **Mock External Services**: Mock external services

## Summary

Spring Boot Integration Testing:
- Test application components together
- Multiple testing strategies
- TestContainers support
- Essential for reliable applications

