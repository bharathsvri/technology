# Code Quality Tools

## Checkstyle

### Setup
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>3.3.1</version>
    <configuration>
        <configLocation>checkstyle.xml</configLocation>
    </configuration>
</plugin>
```

### Usage
```bash
mvn checkstyle:check
```

### Common Rules
- Line length
- Naming conventions
- Import order
- Code formatting
- Method complexity

## PMD

### Setup
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-pmd-plugin</artifactId>
    <version>3.21.0</version>
</plugin>
```

### Usage
```bash
mvn pmd:check
```

### Common Rules
- Unused variables
- Empty catch blocks
- Code duplication
- Complexity
- Best practices

## SpotBugs (FindBugs)

### Setup
```xml
<plugin>
    <groupId>com.github.spotbugs</groupId>
    <artifactId>spotbugs-maven-plugin</artifactId>
    <version>4.8.0.0</version>
</plugin>
```

### Usage
```bash
mvn spotbugs:check
```

### Common Bugs Detected
- Null pointer dereferences
- Resource leaks
- Infinite loops
- Dead code
- Thread safety issues

## SonarQube

### Setup
```xml
<plugin>
    <groupId>org.sonarsource.scanner.maven</groupId>
    <artifactId>sonar-maven-plugin</artifactId>
    <version>3.10.0.2594</version>
</plugin>
```

### Usage
```bash
mvn sonar:sonar
```

### Features
- Code coverage
- Code smells
- Security vulnerabilities
- Technical debt
- Code duplication

## JaCoCo (Code Coverage)

### Setup
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.11</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

### Usage
```bash
mvn test
mvn jacoco:report
```

## Best Practices
1. Run quality checks in CI/CD
2. Set appropriate thresholds
3. Fix issues incrementally
4. Use multiple tools
5. Customize rules for your project
6. Review reports regularly
7. Integrate with build process
8. Track metrics over time

## Complete Maven Configuration
```xml
<build>
    <plugins>
        <!-- Checkstyle -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-checkstyle-plugin</artifactId>
            <version>3.3.1</version>
        </plugin>
        
        <!-- PMD -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-pmd-plugin</artifactId>
            <version>3.21.0</version>
        </plugin>
        
        <!-- SpotBugs -->
        <plugin>
            <groupId>com.github.spotbugs</groupId>
            <artifactId>spotbugs-maven-plugin</artifactId>
            <version>4.8.0.0</version>
        </plugin>
        
        <!-- JaCoCo -->
        <plugin>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.8.11</version>
        </plugin>
    </plugins>
</build>
```

