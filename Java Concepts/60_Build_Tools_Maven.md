# Build Tools - Maven

## What is Maven?
Maven is a build automation and project management tool for Java projects.

## Maven Project Structure
```
project-root/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   └── resources/
│   └── test/
│       ├── java/
│       └── resources/
└── target/
```

## pom.xml (Project Object Model)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <name>My Application</name>
    <description>Description of my application</description>
    
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

## Maven Commands
```bash
# Compile
mvn compile

# Test
mvn test

# Package
mvn package

# Install to local repository
mvn install

# Clean
mvn clean

# Run application
mvn exec:java -Dexec.mainClass="com.example.Main"

# Skip tests
mvn package -DskipTests
```

## Dependency Management
```xml
<dependencies>
    <!-- JUnit for testing -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.0</version>
        <scope>test</scope>
    </dependency>
    
    <!-- SLF4J for logging -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>2.0.9</version>
    </dependency>
    
    <!-- MySQL Connector -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
    </dependency>
</dependencies>
```

## Dependency Scopes
- **compile**: Default, available in all classpaths
- **provided**: Provided by JDK or container
- **runtime**: Needed at runtime, not compile time
- **test**: Only for test compilation and execution
- **system**: Similar to provided, but must provide JAR explicitly

## Maven Lifecycle Phases
1. validate
2. compile
3. test
4. package
5. verify
6. install
7. deploy

## Best Practices
1. Use consistent versioning
2. Manage dependencies carefully
3. Use properties for versions
4. Organize dependencies logically
5. Use appropriate scopes
6. Keep pom.xml clean and readable
7. Use parent POMs for multi-module projects
8. Document project structure

