# Mockito Advanced

## Argument Matchers
```java
import static org.mockito.ArgumentMatchers.*;

when(mock.method(anyString())).thenReturn("result");
when(mock.method(anyInt())).thenReturn(10);
when(mock.method(eq("specific"))).thenReturn("specific result");
when(mock.method(any(User.class))).thenReturn(user);
when(mock.method(argThat(user -> user.getAge() > 18))).thenReturn(true);
```

## Verification
```java
import static org.mockito.Mockito.*;

// Verify method was called
verify(mock).method();

// Verify with times
verify(mock, times(2)).method();
verify(mock, atLeastOnce()).method();
verify(mock, atMost(5)).method();
verify(mock, never()).method();

// Verify with timeout
verify(mock, timeout(1000)).method();

// Verify order
InOrder inOrder = inOrder(mock1, mock2);
inOrder.verify(mock1).method1();
inOrder.verify(mock2).method2();
```

## Stubbing
```java
// Return different values
when(mock.method()).thenReturn("first")
                  .thenReturn("second")
                  .thenThrow(new RuntimeException());

// Answer for complex logic
when(mock.method(anyString())).thenAnswer(invocation -> {
    String arg = invocation.getArgument(0);
    return arg.toUpperCase();
});
```

## Spying
```java
List<String> list = new ArrayList<>();
List<String> spy = spy(list);

// Stub specific method
doReturn(100).when(spy).size();

// Call real method
when(spy.get(0)).thenCallRealMethod();
```

## Best Practices
1. Use argument matchers appropriately
2. Verify important interactions
3. Don't over-mock
4. Use spies for partial mocking
5. Reset mocks when needed
6. Use @InjectMocks for dependencies
7. Keep tests readable
8. Test behavior, not implementation

