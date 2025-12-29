# TestNG

## What is TestNG?
TestNG is a testing framework inspired by JUnit but with additional features.

## Basic Test
```java
import org.testng.annotations.*;

public class TestNGExample {
    @Test
    public void testMethod() {
        Assert.assertEquals(2 + 2, 4);
    }
}
```

## Test Lifecycle
```java
@BeforeSuite
public void beforeSuite() {
    // Runs once before all tests
}

@BeforeTest
public void beforeTest() {
    // Runs before test tag
}

@BeforeClass
public void beforeClass() {
    // Runs before first test in class
}

@BeforeMethod
public void beforeMethod() {
    // Runs before each test method
}

@Test
public void test() {
    // Test code
}

@AfterMethod
public void afterMethod() {
    // Runs after each test method
}
```

## Test Groups
```java
@Test(groups = "smoke")
public void smokeTest() {
    // Smoke test
}

@Test(groups = "regression")
public void regressionTest() {
    // Regression test
}
```

## Data Providers
```java
@DataProvider(name = "testData")
public Object[][] provideData() {
    return new Object[][] {
        {"user1", "password1"},
        {"user2", "password2"}
    };
}

@Test(dataProvider = "testData")
public void testLogin(String username, String password) {
    // Test with different data
}
```

## Parallel Execution
```xml
<suite name="TestSuite" parallel="methods" thread-count="5">
    <test name="Test1">
        <classes>
            <class name="TestNGExample"/>
        </classes>
    </test>
</suite>
```

## Best Practices
1. Use groups for test organization
2. Use data providers for parameterized tests
3. Configure parallel execution
4. Use appropriate assertions
5. Handle test dependencies
6. Use listeners for reporting
7. Organize tests logically
8. Keep tests independent

