# RMI (Remote Method Invocation)

## What is RMI?
RMI allows Java objects to invoke methods on remote objects.

## Remote Interface
```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface Calculator extends Remote {
    int add(int a, int b) throws RemoteException;
    int subtract(int a, int b) throws RemoteException;
    int multiply(int a, int b) throws RemoteException;
}
```

## Remote Implementation
```java
import java.rmi.server.UnicastRemoteObject;
import java.rmi.RemoteException;

public class CalculatorImpl extends UnicastRemoteObject implements Calculator {
    
    public CalculatorImpl() throws RemoteException {
        super();
    }
    
    @Override
    public int add(int a, int b) throws RemoteException {
        return a + b;
    }
    
    @Override
    public int subtract(int a, int b) throws RemoteException {
        return a - b;
    }
    
    @Override
    public int multiply(int a, int b) throws RemoteException {
        return a * b;
    }
}
```

## Server
```java
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class CalculatorServer {
    public static void main(String[] args) {
        try {
            CalculatorImpl calculator = new CalculatorImpl();
            Registry registry = LocateRegistry.createRegistry(1099);
            registry.bind("Calculator", calculator);
            System.out.println("Server ready");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Client
```java
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class CalculatorClient {
    public static void main(String[] args) {
        try {
            Registry registry = LocateRegistry.getRegistry("localhost", 1099);
            Calculator calculator = (Calculator) registry.lookup("Calculator");
            
            int sum = calculator.add(5, 3);
            System.out.println("Sum: " + sum);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Running RMI
```bash
# Start RMI registry
rmiregistry

# Run server
java CalculatorServer

# Run client
java CalculatorClient
```

## Best Practices
1. Handle RemoteException properly
2. Use security manager
3. Use codebase property for dynamic class loading
4. Consider alternatives (REST, gRPC)
5. Handle network failures
6. Use appropriate serialization
7. Secure RMI connections
8. Monitor RMI performance

