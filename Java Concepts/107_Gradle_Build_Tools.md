# Gradle Build Tools

## Why Gradle?
Gradle is a JVM-based build automation tool used widely for Java, Kotlin, Android, and polyglot projects. It complements Maven with a flexible DSL (Groovy or Kotlin), incremental builds, and strong multi-project support.

## Gradle vs Maven (Mental Model)

| Topic | Maven | Gradle |
|-------|--------|--------|
| Model | XML (`pom.xml`) | `build.gradle` / `build.gradle.kts` |
| Conventions | Strong defaults | Strong defaults + easy customization |
| Multi-module | `<modules>` | `include("api", "service")` in `settings.gradle` |
| Dependencies | Coordinates + scopes | Configurations (`implementation`, `api`, `testImplementation`) |
| Plugins | `<plugin>` block | `plugins { id("java") }` or `apply plugin` |

## Minimal Java Library (`build.gradle.kts`)

```kotlin
plugins {
    java
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

dependencies {
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.2")
    testRuntimeOnly("org.junit.platform:junit-platform-launcher")
}

java {
    toolchain {
        languageVersion.set(JavaLanguageVersion.of(21))
    }
}

tasks.test {
    useJUnitPlatform()
}
```

## Key Gradle Concepts

### 1. Settings vs Build Scripts
- **`settings.gradle(.kts)`**: defines project name, included subprojects, plugin management.
- **`build.gradle(.kts)`**: defines plugins, dependencies, tasks for one project.

### 2. Configurations (Dependency Scopes)
- **`implementation`**: on compile and runtime classpaths; not leaked to consumers.
- **`api`**: like `implementation`, but exposed to consumers (Java library plugin).
- **`compileOnly`**: compile-time only (e.g., annotations processors, provided APIs).
- **`runtimeOnly`**: runtime only (e.g., JDBC drivers in some setups).
- **`testImplementation`**: tests only.

### 3. Tasks and Incremental Builds
- Everything is a **task** (`compileJava`, `test`, `jar`, `bootJar` with Spring plugin).
- Gradle tracks inputs/outputs to skip work when nothing changed.

### 4. Wrapper (`gradlew`)
Commit the wrapper so everyone uses the same Gradle version:
- `gradle wrapper --gradle-version 8.10`

## Common Commands

```bash
./gradlew build          # compile, test, package
./gradlew test           # tests only
./gradlew dependencies   # dependency insight
./gradlew clean build    # full rebuild
./gradlew projects       # multi-module tree
```

## Spring Boot with Gradle
Use the Spring Boot Gradle plugin to create executable jars, manage dependency BOMs, and run the app with `./gradlew bootRun`.

## Best Practices
- Prefer **Kotlin DSL** (`build.gradle.kts`) for autocomplete and type safety.
- Use **Java toolchains** to compile/test against a fixed JDK without relying on `JAVA_HOME` alone.
- Keep **plugins** and **versions** centralized (`gradle/libs.versions.toml` with version catalogs).
- Avoid heavy configuration in `allprojects`; prefer convention plugins for large repos.

## When to Choose Gradle
- Large multi-repo or monorepo-style builds needing custom pipelines.
- Teams wanting programmatic build logic without XML.
- Android or Kotlin-first stacks (first-class support).

## Further Reading
- Gradle User Manual: dependency management, task authoring, performance tuning (build cache).
