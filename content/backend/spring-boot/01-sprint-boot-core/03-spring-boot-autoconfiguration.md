---
{"publish":true,"title":"Spring Boot Autoconfiguration","cssclasses":""}
---

## Definitions

**Autoconfiguration**: Spring Boot mechanism automatically creating and configuring beans based on classpath dependencies and properties. Reduces need for manual bean definitions and configuration.

**@EnableAutoConfiguration**: Annotation enabling Spring Boot's autoconfiguration. Replaced by `@SpringBootApplication` in modern Spring Boot. Tells Spring Boot to guess and configure beans based on jar dependencies.

**@ConditionalOnClass**: Conditional annotation; bean created only if specified class is present on classpath. Used for conditional configuration.

**@ConditionalOnMissingBean**: Bean created only if specified bean type not already defined in application context. Prevents overriding user-defined beans.

**Condition**: Spring mechanism evaluating whether a bean should be registered. Enables flexible, profile-aware, environment-specific configuration.

**META-INF/spring.factories**: Configuration file (or `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` in Spring Boot 2.7+) listing autoconfiguration classes. Enables Spring to discover and load autoconfiguration.

---

## Q&A

**How does Spring Boot autoconfiguration work?**

Autoconfiguration scans classpath for libraries, reads properties from `application.properties`/`application.yml`, then conditionally creates beans matching detected dependencies. For example, if `spring-boot-starter-data-jpa` is present, `DataSource` and `EntityManagerFactory` beans are auto-created. Conditions like `@ConditionalOnClass` and `@ConditionalOnMissingBean` determine actual bean creation.

**Can you disable autoconfiguration for specific features?**

Yes, set `spring.autoconfigure.exclude` property to exclude specific autoconfiguration classes. Example: `spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration`. Alternatively, use `@EnableAutoConfiguration(exclude={...})` annotation. This is useful when you want to provide custom bean definitions.

**How do you create custom autoconfiguration?**

Create a `@Configuration` class with conditional annotations (`@ConditionalOnClass`, `@ConditionalOnMissingBean`), then register it in `META-INF/spring.factories` or `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`. Spring Boot will discover and apply it during application startup if conditions are met.

**What's the difference between @ConditionalOnClass and @ConditionalOnMissingClass?**

`@ConditionalOnClass` creates bean only if specified class exists on classpath. `@ConditionalOnMissingClass` creates bean only if class does NOT exist on classpath. Use to provide fallback implementations or avoid conflicts with optional dependencies.

**How do you override autoconfigured beans?**

Define your own bean in `@Configuration` class or via `@Bean` method. Your custom bean takes precedence over autoconfigured bean (assuming `@ConditionalOnMissingBean` on autoconfiguration). Alternatively, use properties to customize autoconfigured beans without overriding entirely.

**What happens if autoconfiguration conflicts with your config?**

Your configuration takes priority. Autoconfiguration includes conditions ensuring it doesn't override explicitly defined beans. If you define custom `DataSource` bean, autoconfigured `DataSource` is skipped. You can inspect autoconfiguration report to understand what was configured.

---

## Code Examples

**Example - Custom Autoconfiguration Class:**
```java
@Configuration
@ConditionalOnClass(MyService.class)
@ConditionalOnMissingBean(MyService.class)
@EnableConfigurationProperties(MyServiceProperties.class)
public class MyServiceAutoconfiguration {
    
    @Bean
    public MyService myService(MyServiceProperties props) {
        return new MyService(props.getName(), props.getTimeout());
    }
}
```

**Example - Properties for Autoconfiguration:**
```java
@ConfigurationProperties(prefix = "myapp.service")
public class MyServiceProperties {
    private String name = "default";
    private int timeout = 5000;
    // getters and setters
}
```

**Example - Registering Autoconfiguration (META-INF/spring.factories):**
```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.MyServiceAutoconfiguration
```

**Example - Disabling Autoconfiguration:**
```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

**Example - Application YAML Override:**
```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: validate  # Override autoconfigured ddl-auto
  datasource:
    url: jdbc:mysql://custom-host:3306/db  # Custom datasource
```

---

## Key Points

- **Convention-Based**: Autoconfiguration assumes standard setups; override only as needed.
- **Conditional Stacking**: Multiple conditions can be combined for fine-grained control (AND logic).
- **Inspection**: Run with `--debug` flag or check `DEBUG=true` log output to see autoconfiguration report.
- **Order Matters**: Autoconfiguration classes load in specific order; use `@AutoConfigureAfter`/`@AutoConfigureBefore` to control sequence.
- **Spring Boot 2.7+**: New location for autoconfiguration discovery (`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`) recommended over older `META-INF/spring.factories`.
