# Module System (Java 9+)

## What are Modules?
Modules provide strong encapsulation and explicit dependencies. They help create more maintainable and scalable applications.

## Module Declaration (module-info.java)
```java
module com.example.app {
    requires java.base;
    requires java.logging;
    exports com.example.util;
    exports com.example.model to com.example.client;
}
```

## Basic Module Structure
```
myapp/
├── src/
│   ├── com.example.app/
│   │   ├── module-info.java
│   │   └── com/
│   │       └── example/
│   │           └── app/
│   │               └── Main.java
```

## Module Descriptors

### Requires
```java
module com.example.app {
    requires java.base;        // Implicit, always available
    requires java.logging;
    requires java.sql;
    requires transitive com.example.util; // Transitive dependency
}
```

### Exports
```java
module com.example.util {
    exports com.example.util;
    exports com.example.util.helpers;
    exports com.example.util.internal; // Internal API
}
```

### Opens
```java
module com.example.app {
    opens com.example.model; // Opens for reflection
    opens com.example.model to com.example.test; // Opens to specific module
}
```

### Provides and Uses
```java
module com.example.service {
    provides com.example.service.Service 
        with com.example.service.ServiceImpl;
    
    uses com.example.service.Service;
}
```

## Compiling Modules
```bash
# Compile module
javac -d mods/com.example.app \
    src/com.example.app/module-info.java \
    src/com.example.app/com/example/app/Main.java

# Run module
java --module-path mods -m com.example.app/com.example.app.Main
```

## Module Path
```bash
# Set module path
java --module-path mods -m com.example.app/com.example.app.Main

# Multiple module paths
java --module-path mods1:mods2 -m com.example.app/com.example.app.Main
```

## JAR as Module
```bash
# Create modular JAR
jar --create --file=app.jar --main-class=com.example.app.Main \
    -C mods/com.example.app .

# Run modular JAR
java -p app.jar -m com.example.app
```

## Service Provider Pattern
```java
// Service interface
package com.example.service;
public interface Service {
    void execute();
}

// Service implementation
package com.example.service.impl;
public class ServiceImpl implements Service {
    @Override
    public void execute() {
        System.out.println("Service executed");
    }
}

// Module descriptor
module com.example.service {
    provides com.example.service.Service 
        with com.example.service.impl.ServiceImpl;
}

// Consumer module
module com.example.client {
    requires com.example.service;
    uses com.example.service.Service;
}

// Using service
ServiceLoader<Service> loader = ServiceLoader.load(Service.class);
Service service = loader.findFirst().orElseThrow();
service.execute();
```

## Best Practices
1. Use modules for large applications
2. Keep module boundaries clear
3. Use transitive requires carefully
4. Export only necessary packages
5. Use opens for reflection when needed
6. Document module dependencies
7. Test module boundaries
8. Use service provider for extensibility

