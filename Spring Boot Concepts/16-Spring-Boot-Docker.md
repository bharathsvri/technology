# Spring Boot Docker

## What is Docker?

Docker is a platform for developing, shipping, and running applications using containerization. It packages applications with all dependencies into containers.

## Dockerizing Spring Boot Application

### Create Dockerfile

```dockerfile
# Use OpenJDK as base image
FROM openjdk:17-jdk-slim

# Set working directory
WORKDIR /app

# Copy Maven wrapper and pom.xml
COPY .mvn/ .mvn
COPY mvnw pom.xml ./

# Download dependencies
RUN ./mvnw dependency:go-offline

# Copy source code
COPY src ./src

# Build application
RUN ./mvnw clean package -DskipTests

# Run application
ENTRYPOINT ["java", "-jar", "target/application.jar"]
```

### Multi-Stage Dockerfile

```dockerfile
# Stage 1: Build
FROM maven:3.8.6-openjdk-17 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Optimized Dockerfile

```dockerfile
FROM eclipse-temurin:17-jre-alpine AS builder
WORKDIR /app
COPY target/*.jar app.jar
RUN java -Djarmode=layertools -jar app.jar extract

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder app/dependencies/ ./
COPY --from=builder app/spring-boot-loader/ ./
COPY --from=builder app/snapshot-dependencies/ ./
COPY --from=builder app/application/ ./
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
```

## Docker Compose

### docker-compose.yml

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:mysql://db:3306/mydb
      - SPRING_DATASOURCE_USERNAME=root
      - SPRING_DATASOURCE_PASSWORD=password
    depends_on:
      - db
    networks:
      - app-network

  db:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=mydb
    ports:
      - "3306:3306"
    volumes:
      - db-data:/var/lib/mysql
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    networks:
      - app-network

volumes:
  db-data:

networks:
  app-network:
    driver: bridge
```

## Building and Running

### Build Docker Image

```bash
docker build -t my-spring-boot-app .
```

### Run Container

```bash
docker run -p 8080:8080 my-spring-boot-app
```

### Using Docker Compose

```bash
docker-compose up -d
```

## Spring Boot Docker Plugin

### Add Plugin

```xml
<plugin>
    <groupId>com.google.cloud.tools</groupId>
    <artifactId>jib-maven-plugin</artifactId>
    <version>3.3.1</version>
    <configuration>
        <to>
            <image>my-spring-boot-app:latest</image>
        </to>
    </configuration>
</plugin>
```

### Build with Jib

```bash
mvn compile jib:dockerBuild
```

## Environment Variables

### Using Environment Variables

```dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/*.jar app.jar
ENV SPRING_PROFILES_ACTIVE=prod
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Docker Compose Environment

```yaml
services:
  app:
    build: .
    environment:
      - SPRING_PROFILES_ACTIVE=prod
      - SPRING_DATASOURCE_URL=jdbc:mysql://db:3306/mydb
    env_file:
      - .env
```

## Health Checks

### Docker Health Check

```dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/*.jar app.jar
HEALTHCHECK --interval=30s --timeout=3s --start-period=40s --retries=3 \
  CMD curl -f http://localhost:8080/actuator/health || exit 1
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## Best Practices

1. **Use Multi-Stage Builds**: Reduce image size
2. **Use .dockerignore**: Exclude unnecessary files
3. **Layer Caching**: Order Dockerfile commands for better caching
4. **Use Specific Tags**: Avoid using `latest` tag
5. **Minimize Layers**: Combine RUN commands
6. **Use Alpine Images**: Smaller base images
7. **Set Health Checks**: Monitor container health
8. **Use Secrets**: Don't hardcode secrets

## Summary

Spring Boot Docker:
- Easy to containerize Spring Boot applications
- Supports multi-stage builds
- Works with Docker Compose
- Essential for containerized deployments

