# Testing Basics

## JUnit Basics

### Setup
```xml
<!-- Maven dependency -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

### Basic Test
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {
    @Test
    void testAdd() {
        Calculator calc = new Calculator();
        int result = calc.add(5, 3);
        assertEquals(8, result);
    }
}
```

### Assertions
```java
import static org.junit.jupiter.api.Assertions.*;

@Test
void testAssertions() {
    // Equality
    assertEquals(5, 5);
    assertNotEquals(5, 6);
    
    // Null checks
    assertNull(null);
    assertNotNull("value");
    
    // Boolean
    assertTrue(true);
    assertFalse(false);
    
    // Arrays
    assertArrayEquals(new int[]{1, 2, 3}, new int[]{1, 2, 3});
    
    // Exceptions
    assertThrows(IllegalArgumentException.class, () -> {
        throw new IllegalArgumentException();
    });
}
```

### Test Lifecycle
```java
import org.junit.jupiter.api.*;

class LifecycleTest {
    @BeforeAll
    static void setUpAll() {
        // Runs once before all tests
    }
    
    @BeforeEach
    void setUp() {
        // Runs before each test
    }
    
    @Test
    void test1() {
        // Test code
    }
    
    @AfterEach
    void tearDown() {
        // Runs after each test
    }
    
    @AfterAll
    static void tearDownAll() {
        // Runs once after all tests
    }
}
```

### Parameterized Tests
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

@ParameterizedTest
@ValueSource(ints = {1, 2, 3, 4, 5})
void testIsPositive(int number) {
    assertTrue(number > 0);
}
```

### Test Organization
```java
import org.junit.jupiter.api.*;

@DisplayName("Calculator Tests")
class CalculatorTest {
    
    @Nested
    @DisplayName("Addition Tests")
    class AdditionTests {
        @Test
        void testAddPositive() {
            Calculator calc = new Calculator();
            assertEquals(5, calc.add(2, 3));
        }
        
        @Test
        void testAddNegative() {
            Calculator calc = new Calculator();
            assertEquals(-1, calc.add(-3, 2));
        }
    }
}
```

## Mocking (Mockito)

### Setup
```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>5.6.0</version>
    <scope>test</scope>
</dependency>
```

### Basic Mocking
```java
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import static org.mockito.Mockito.*;

class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
    }
    
    @Test
    void testGetUser() {
        User user = new User("John", 25);
        when(userRepository.findById(1L)).thenReturn(user);
        
        UserService service = new UserService(userRepository);
        User result = service.getUser(1L);
        
        assertEquals("John", result.getName());
        verify(userRepository).findById(1L);
    }
}
```

## Test Best Practices
1. Write tests before or alongside code (TDD)
2. Test one thing per test method
3. Use descriptive test names
4. Keep tests independent
5. Use setup/teardown methods
6. Test edge cases and error conditions
7. Mock external dependencies
8. Maintain test code quality
9. Run tests frequently
10. Aim for high code coverage

## Complete Example
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class BankAccountTest {
    private BankAccount account;
    
    @BeforeEach
    void setUp() {
        account = new BankAccount(100.0);
    }
    
    @Test
    @DisplayName("Deposit increases balance")
    void testDeposit() {
        account.deposit(50.0);
        assertEquals(150.0, account.getBalance());
    }
    
    @Test
    @DisplayName("Withdraw decreases balance")
    void testWithdraw() {
        account.withdraw(30.0);
        assertEquals(70.0, account.getBalance());
    }
    
    @Test
    @DisplayName("Withdraw throws exception when insufficient funds")
    void testWithdrawInsufficientFunds() {
        assertThrows(InsufficientFundsException.class, () -> {
            account.withdraw(200.0);
        });
    }
    
    @Test
    @DisplayName("Negative deposit throws exception")
    void testNegativeDeposit() {
        assertThrows(IllegalArgumentException.class, () -> {
            account.deposit(-10.0);
        });
    }
}
```

