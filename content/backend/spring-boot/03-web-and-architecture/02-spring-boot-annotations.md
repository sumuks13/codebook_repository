---
{"publish":true,"title":"Spring Boot Annotations","cssclasses":""}
---

## Definitions

**@SpringBootApplication**: Meta-annotation combining @Configuration, @ComponentScan, @EnableAutoConfiguration. Marks class as Spring Boot entry point.

**@Component**: Generic stereotype annotation marking class as Spring component. Automatically detected by component scanning, registered as bean.

**@Service**: Specialization of @Component for service layer classes. Indicates class contains business logic.

**@Repository**: Specialization of @Component for data access layer. Marks class as data repository, enables exception translation from database to Spring exceptions.

**@Controller**: Specialization of @Component for MVC layer. Marks class handling HTTP requests, combined with @RequestMapping for route handling.

**@RestController**: Combination of @Controller and @ResponseBody. Returns JSON/XML directly instead of view name.

**@Configuration**: Marks class containing @Bean definitions. Beans defined in methods marked with @Bean. Equivalent to XML `<beans>` element.

**@Bean**: Method annotation indicating method returns bean to be registered in container. Used in @Configuration classes. Equivalent to XML `<bean>` element.

**@Autowired**: Marks field, setter, or constructor for dependency injection. Spring resolves and injects dependency automatically.

**@Qualifier**: Disambiguates multiple beans of same type. Specifies which bean to inject when multiple candidates exist.

---

## Q&A

**Why use stereotype annotations like @Service instead of @Component?**

@Service, @Repository, @Controller are semantic; code intent is clearer. @Repository enables database exception translation. Future Spring versions may add specialized behavior to specific stereotypes. For consistency and clarity, use appropriate stereotype for each layer rather than generic @Component.

**What's the difference between @Bean and @Component?**

@Component marks classes for component scanning; Spring instantiates and registers automatically. @Bean marks methods in @Configuration classes; developer controls instantiation logic. Use @Component for your own classes; @Bean for external library classes or when complex initialization needed.

**How do you specify bean initialization order?**

Use `@Order` annotation on @Configuration classes or @Bean methods. Lower numbers execute first; default is LOWEST_PRECEDENCE. Alternatively, use `@DependsOn("beanName")` making bean depend on another, enforcing initialization order.

**Can @Autowired inject null safely?**

Use `Optional<T>` wrapper for safe null handling: `@Autowired private Optional<Service> service;` then `service.ifPresent(...)`. Or use `@Autowired(required=false)` allowing null injection; check for null before use. Optional clearer about possible null; `required=false` can hide configuration errors.

**What's the purpose of @ConfigurationProperties?**

@ConfigurationProperties binds external configuration (properties, YAML) to strongly-typed class. Prefix parameter groups related properties. Example: `@ConfigurationProperties(prefix="app.database")` creates bean with properties from `app.database.*`. Better than @Value for groups of related properties.

**How do you make @Configuration conditional?**

Use @ConditionalOnClass, @ConditionalOnMissingBean, @ConditionalOnProperty. Example: `@ConditionalOnProperty(name="feature.enabled", havingValue="true")` creates beans only if property true. Enables feature flags, optional features based on classpath, missing beans.

---

## Code Examples

**Example - Service Layer Annotation:**
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public User getUserById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
    }
}
```

**Example - @Bean Configuration:**
```java
@Configuration
public class AppConfig {
    
    @Bean
    public UserRepository userRepository() {
        return new UserRepositoryImpl();
    }
    
    @Bean
    public UserService userService(UserRepository repository) {
        return new UserService(repository);
    }
}
```

**Example - @ConfigurationProperties:**
```java
@Configuration
@ConfigurationProperties(prefix = "app.datasource")
@Getter @Setter
public class DataSourceProperties {
    private String url;
    private String username;
    private String password;
    private int poolSize = 10;
}

// application.yml
// app:
//   datasource:
//     url: jdbc:mysql://localhost/db
//     username: root
//     password: password
```

**Example - Conditional Configuration:**
```java
@Configuration
@ConditionalOnProperty(
    name = "feature.caching.enabled",
    havingValue = "true",
    matchIfMissing = false
)
public class CachingConfig {
    
    @Bean
    public CacheManager cacheManager() {
        return new RedisCacheManager();
    }
}
```

**Example - Optional Autowiring:**
```java
@Component
public class FeatureService {
    
    @Autowired
    private Optional<AdvancedFeature> advancedFeature;
    
    public void process() {
        advancedFeature.ifPresentOrElse(
            feature -> feature.doAdvancedProcessing(),
            () -> doBasicProcessing()
        );
    }
}
```

---

## Key Points

- **Annotation Scanning**: @ComponentScan searches packages for @Component stereotypes. Default scans package containing @SpringBootApplication.
- **Meta-annotations**: Stereotypes are meta-annotated with @Component; framework recognizes and treats specially.
- **Bean Naming**: Default bean name is class name (first letter lowercased). Override with `@Service("customName")`.
- **Lazy Loading**: Use @Lazy to defer bean instantiation until first access, reducing startup time.
- **Scope with Stereotypes**: Combine @Service with @Scope("prototype") for prototype-scoped services.
