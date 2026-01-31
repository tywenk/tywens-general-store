# Java Stack Profile

## Import Patterns
- Wildcard imports (`import java.util.*`) vs specific — check style consistency
- Check for unused imports — IDE neglect indicator
- Internal sun/com packages — fragile dependencies

## Build System
- `pom.xml` → Maven
- `build.gradle` / `build.gradle.kts` → Gradle (check Groovy vs Kotlin DSL migration)
- Both present → migration in progress
- Check for multi-module structure

## Framework Signals
- Spring Boot (`@SpringBootApplication`) — check version for era
- Jakarta EE vs javax namespace → Java EE migration
- Hibernate/JPA annotations
- Check for reactive (WebFlux) vs servlet (MVC) patterns

## Legacy Indicators
- Java version in pom/gradle (8 vs 11 vs 17 vs 21)
- `javax.*` packages alongside `jakarta.*` → namespace migration
- XML config alongside annotation-based config → Spring migration
- Raw types (no generics) → very old code
- Manual resource management vs try-with-resources → era indicator
- Synchronized blocks vs java.util.concurrent → concurrency era

## Configuration Surfaces
- `application.properties` / `application.yml` (Spring)
- `persistence.xml` (JPA)
- `web.xml` (servlet config — legacy)
- `logback.xml` / `log4j2.xml`
- Environment-specific configs (profiles)

## Testing Conventions
- JUnit 4 (`@Test` from org.junit) vs JUnit 5 (`@Test` from org.junit.jupiter)
- Mockito patterns — check for consistent mocking style
- Integration test separation from unit tests
- TestContainers usage

## Common Migration Patterns
- Java 8 → 11+: Look for removed APIs, module system adoption
- javax → jakarta: Namespace migration for EE APIs
- Spring XML → annotation config → Spring Boot
- JUnit 4 → 5: Look for mixed test annotations
- Monolith → microservices: Look for shared libraries, service boundaries
