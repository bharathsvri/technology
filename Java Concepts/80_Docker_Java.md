# Docker with Java

## Dockerfile for Java Application
```dockerfile
# Use official OpenJDK image
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
RUN ./mvnw package -DskipTests

# Run application
ENTRYPOINT ["java", "-jar", "target/myapp.jar"]
```

## Multi-Stage Build
```dockerfile
# Build stage
FROM maven:3.8-openjdk-17 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM openjdk:17-jre-slim
WORKDIR /app
COPY --from=build /app/target/myapp.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## Docker Compose
```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=prod
      - DATABASE_URL=jdbc:mysql://db:3306/mydb
    depends_on:
      - db
  
  db:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_DATABASE=mydb
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

## Building and Running
```bash
# Build image
docker build -t my-java-app .

# Run container
docker run -p 8080:8080 my-java-app

# Run with environment variables
docker run -p 8080:8080 -e SPRING_PROFILES_ACTIVE=dev my-java-app

# Run with volume
docker run -p 8080:8080 -v /data:/app/data my-java-app
```

## Best Practices
1. Use multi-stage builds
2. Use .dockerignore
3. Minimize image size
4. Use specific base image versions
5. Set appropriate JVM options
6. Use health checks
7. Handle signals properly
8. Use secrets for sensitive data
9. Optimize layer caching
10. Use appropriate base images

