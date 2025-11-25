---
{"publish":true,"title":"Spring Boot Basics: Introduction and Configuration","cssclasses":""}
---

## Definitions

**Spring Boot**: Open-source Java framework for creating stand-alone, production-grade applications with minimal configuration. Emphasizes convention over configuration and provides embedded servers.

**Autoconfiguration**: Spring Boot mechanism automatically configuring Spring application based on jar dependencies present on classpath. Uses `@ConditionalOnClass`, `@ConditionalOnMissingBean` to conditionally create beans.

**Starter Dependencies**: Pre-configured dependency descriptors following `spring-boot-starter-*` naming convention. Aggregates related libraries into single dependency for rapid prototyping.

**Embedded Server**: Web server (Tomcat, Jetty, Undertow) embedded within Spring Boot application. Eliminates need for external application server; application runs as standalone JAR.

**Spring Profiles**: Mechanism for managing environment-specific configuration (dev, test, prod). Activated via `spring.profiles.active` property, enabling selective bean creation and configuration loading.

**application.properties/application.yml**: Configuration files containing application settings. YAML format offers cleaner syntax; properties format offers simplicity. YAML preferred for readability.

---

## Q&A

**Why is Spring Boot preferred over traditional Spring?**

Spring Boot eliminates extensive XML configuration, provides built-in dependency management through starters, includes embedded servers, and offers production-ready monitoring features. Traditional Spring requires explicit setup for most features; Spring Boot uses opinionated defaults reducing boilerplate significantly.

**How does Spring Boot simplify application startup?**

Spring Boot leverages starters, automatic component scanning via `@SpringBootApplication`, and auto-configuration to set up beans with sensible defaults. Applications start with a single `SpringApplication.run()` call in the main method. The embedded server eliminates external server deployment steps.

**What is the typical directory structure of a Spring Boot project?**

Spring Boot projects follow standard Maven/Gradle structure: `src/main/java/` for source code, `src/main/resources/` for configuration files and templates, `src/test/java/` for tests. Configuration files like `application.properties` and static assets reside in `src/main/resources/`.

**How do you configure application properties?**

Application configuration is managed through `application.properties` or `application.yml` in `src/main/resources/`. Common settings include server port, database URLs, logging levels. Example: `server.port=8081` or profile-specific files like `application-dev.properties`.

**How do you enable different environments (dev/prod) in Spring Boot?**

Create environment-specific files: `application-dev.properties`, `application-prod.properties`. Activate using `spring.profiles.active=dev` in main config or as startup argument `--spring.profiles.active=prod`. Conditional bean creation uses `@Profile("dev")` annotation.

**What does @SpringBootApplication annotation do?**

Meta-annotation combining `@Configuration`, `@ComponentScan`, and `@EnableAutoConfiguration`. Marks class as Spring Boot application entry point, enables component scanning from package and sub-packages, and triggers auto-configuration based on classpath dependencies.

---

## Code Examples

**Example - Simple Application Startup:**
```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

**Example - application.properties Configuration:**
```
server.port=9090
spring.application.name=demo-app
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
logging.level.root=INFO
```

**Example - application.yml Configuration:**
```yaml
server:
  port: 9090
spring:
  application:
    name: demo-app
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: password
logging:
  level:
    root: INFO
```

**Example - Profile-Specific Configuration (application-dev.properties):**
```
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/dev_db
logging.level.root=DEBUG
```

---

## Key Points

- **Convention Over Configuration**: Spring Boot assumes typical use patterns; override only when necessary.
- **Embedded Server**: Single JAR file contains everything; deploy with `java -jar app.jar`.
- **Profile Support**: Enables flexible configuration across development, testing, and production environments.
- **Spring Initializr**: Use https://start.spring.io for quick project scaffolding with pre-configured dependencies.
- **Minimal XML**: Strongly favor annotation and code-based configuration over XML.
