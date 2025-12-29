# Logging

## What is Logging?
Logging is the process of recording application events and messages for debugging, monitoring, and auditing purposes.

## Java Logging API (java.util.logging)

### Basic Logging
```java
import java.util.logging.Logger;
import java.util.logging.Level;

public class LoggingExample {
    private static final Logger logger = Logger.getLogger(LoggingExample.class.getName());
    
    public static void main(String[] args) {
        logger.info("Application started");
        logger.warning("This is a warning");
        logger.severe("This is an error");
    }
}
```

### Log Levels
```java
import java.util.logging.*;

Logger logger = Logger.getLogger("MyLogger");

logger.severe("Severe message");    // Highest priority
logger.warning("Warning message");
logger.info("Info message");
logger.config("Config message");
logger.fine("Fine message");
logger.finer("Finer message");
logger.finest("Finest message");   // Lowest priority
```

### Logging Configuration
```java
import java.util.logging.*;

// Programmatic configuration
Logger logger = Logger.getLogger("MyLogger");
logger.setLevel(Level.INFO);

// Create console handler
ConsoleHandler consoleHandler = new ConsoleHandler();
consoleHandler.setLevel(Level.INFO);
logger.addHandler(consoleHandler);

// Create file handler
try {
    FileHandler fileHandler = new FileHandler("app.log");
    fileHandler.setLevel(Level.ALL);
    logger.addHandler(fileHandler);
} catch (IOException e) {
    e.printStackTrace();
}
```

### Custom Formatters
```java
import java.util.logging.*;

class CustomFormatter extends Formatter {
    @Override
    public String format(LogRecord record) {
        return String.format("[%s] %s: %s%n",
            record.getLevel(),
            record.getLoggerName(),
            record.getMessage());
    }
}

// Usage
Logger logger = Logger.getLogger("MyLogger");
ConsoleHandler handler = new ConsoleHandler();
handler.setFormatter(new CustomFormatter());
logger.addHandler(handler);
```

## SLF4J (Simple Logging Facade)

### Setup
```xml
<!-- Maven dependency -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.9</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>2.0.9</version>
</dependency>
```

### Basic Usage
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class SLF4JExample {
    private static final Logger logger = LoggerFactory.getLogger(SLF4JExample.class);
    
    public static void main(String[] args) {
        logger.trace("Trace message");
        logger.debug("Debug message");
        logger.info("Info message");
        logger.warn("Warning message");
        logger.error("Error message", new Exception("Error occurred"));
    }
}
```

### Parameterized Logging
```java
String name = "John";
int age = 25;

// Efficient - only evaluated if log level is enabled
logger.debug("User: {} is {} years old", name, age);

// Avoid string concatenation
logger.debug("User: " + name + " is " + age + " years old"); // Inefficient
```

## Log4j 2

### Setup
```xml
<!-- Maven dependency -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.20.0</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.20.0</version>
</dependency>
```

### Basic Usage
```java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class Log4jExample {
    private static final Logger logger = LogManager.getLogger(Log4jExample.class);
    
    public static void main(String[] args) {
        logger.trace("Trace message");
        logger.debug("Debug message");
        logger.info("Info message");
        logger.warn("Warning message");
        logger.error("Error message");
        logger.fatal("Fatal message");
    }
}
```

### Configuration (log4j2.xml)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n"/>
        </Console>
        <File name="File" fileName="app.log">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n"/>
        </File>
    </Appenders>
    <Loggers>
        <Root level="info">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="File"/>
        </Root>
    </Loggers>
</Configuration>
```

## Best Practices
1. Use appropriate log levels
2. Include context in log messages
3. Use parameterized logging (avoid string concatenation)
4. Don't log sensitive information
5. Use structured logging for production
6. Configure log rotation
7. Set appropriate log levels per environment
8. Use MDC (Mapped Diagnostic Context) for correlation

## Log Levels Comparison

| Level | Java Logging | SLF4J | Log4j2 | Priority |
|-------|--------------|-------|--------|----------|
| Most detailed | FINEST | TRACE | TRACE | Lowest |
| Debug info | FINE | DEBUG | DEBUG | |
| General info | INFO | INFO | INFO | |
| Warnings | WARNING | WARN | WARN | |
| Errors | SEVERE | ERROR | ERROR | |
| Critical | | | FATAL | Highest |

## MDC (Mapped Diagnostic Context)
```java
import org.slf4j.MDC;

// Set context
MDC.put("userId", "12345");
MDC.put("requestId", "req-001");

logger.info("Processing request");

// Clear context
MDC.clear();
```

## Complete Example
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.slf4j.MDC;

public class LoggingDemo {
    private static final Logger logger = LoggerFactory.getLogger(LoggingDemo.class);
    
    public static void main(String[] args) {
        MDC.put("sessionId", "session-123");
        
        logger.info("Application started");
        
        try {
            processData();
        } catch (Exception e) {
            logger.error("Error processing data", e);
        }
        
        MDC.clear();
        logger.info("Application ended");
    }
    
    private static void processData() {
        logger.debug("Processing data");
        // Processing logic
    }
}
```

