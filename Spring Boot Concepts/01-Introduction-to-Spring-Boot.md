# Introduction to Spring Boot

## What is Spring Boot?

Spring Boot is an open-source Java-based framework used to create microservices and standalone applications. It is built on top of the Spring framework and provides a production-ready environment with minimal configuration.

## Key Features

### 1. **Auto Configuration**
- Automatically configures Spring and third-party libraries based on classpath
- Reduces boilerplate code and configuration

### 2. **Standalone Applications**
- Embedded servers (Tomcat, Jetty, Undertow)
- No need for external server deployment
- Run as JAR files

### 3. **Production Ready**
- Built-in health checks, metrics, and monitoring
- Actuator endpoints for production monitoring

### 4. **Opinionated Defaults**
- Provides sensible defaults that can be overridden
- Convention over configuration approach

### 5. **No Code Generation**
- No XML configuration required
- Pure Java-based configuration

## Spring Boot vs Spring Framework

| Feature | Spring Framework | Spring Boot |
|---------|-----------------|-------------|
| Configuration | XML/Java Config | Auto Configuration |
| Server | External | Embedded |
| Deployment | WAR file | JAR file |
| Setup Time | More | Less |
| Boilerplate Code | More | Less |

## Spring Boot Architecture

```
┌─────────────────────────────────────┐
│     Spring Boot Application         │
├─────────────────────────────────────┤
│  Spring Boot Auto Configuration     │
├─────────────────────────────────────┤
│  Spring Framework Core              │
├─────────────────────────────────────┤
│  Embedded Server (Tomcat/Jetty)     │
└─────────────────────────────────────┘
```

## Creating a Spring Boot Application

### Method 1: Using Spring Initializr

1. Visit https://start.spring.io
2. Select project metadata (Group, Artifact, Dependencies)
3. Generate and download the project
4. Extract and import into IDE

### Method 2: Using Maven

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
    <relativePath/>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

### Method 3: Using Gradle

```gradle
plugins {
    id 'org.springframework.boot' version '3.2.0'
    id 'io.spring.dependency-management' version '1.1.4'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

## Basic Spring Boot Application Structure

```
src/
├── main/
│   ├── java/
│   │   └── com/
│   │       └── example/
│   │           └── Application.java
│   └── resources/
│       ├── application.properties
│       └── static/
└── test/
    └── java/
```

## Main Application Class

```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### @SpringBootApplication Annotation

This annotation is a combination of:
- `@Configuration`: Marks the class as a configuration class
- `@EnableAutoConfiguration`: Enables Spring Boot auto-configuration
- `@ComponentScan`: Scans for components in the package and sub-packages

## Running Spring Boot Application

### Method 1: Using IDE
- Right-click on the main class → Run As → Java Application

### Method 2: Using Maven
```bash
mvn spring-boot:run
```

### Method 3: Using Gradle
```bash
./gradlew bootRun
```

### Method 4: Using JAR
```bash
java -jar target/application.jar
```

## Advantages of Spring Boot

1. **Rapid Development**: Quick setup and development
2. **Less Configuration**: Auto-configuration reduces setup time
3. **Embedded Server**: No need for external server
4. **Production Ready**: Built-in monitoring and metrics
5. **Microservices Ready**: Perfect for microservices architecture
6. **Large Community**: Extensive community support
7. **Spring Ecosystem**: Access to entire Spring ecosystem

## Disadvantages

1. **Less Control**: Auto-configuration may hide complexity
2. **Large JAR Size**: Can create large deployment files
3. **Learning Curve**: Need to understand Spring Boot conventions

## Spring Boot Versions

- **Spring Boot 1.x**: Java 7/8, Servlet 3.1+
- **Spring Boot 2.x**: Java 8+, Servlet 4.0+
- **Spring Boot 3.x**: Java 17+, Jakarta EE 9+

## System Requirements

### Spring Boot 3.x
- Java 17 or later
- Spring Framework 6.0+
- Maven 3.5+ or Gradle 7.x+

## Next Steps

After understanding the basics, explore:
- Spring Boot Starters
- Auto Configuration
- Application Properties
- Profiles
- Actuator

