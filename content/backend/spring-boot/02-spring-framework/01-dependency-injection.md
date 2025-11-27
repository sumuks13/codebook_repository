---
{"publish":true,"title":"Dependency Injection","cssclasses":""}
---

## Definitions

**Dependency Injection (DI)**: Design pattern where objects receive dependencies from external sources rather than creating them internally. Promotes loose coupling and testability.

**Autowiring**: Spring mechanism automatically injecting beans into class members (constructor, setter, field). Occurs via `@Autowired`, `@Inject`, or constructor injection.

**Qualifier**: Mechanism disambiguating multiple beans of same type. Used with `@Qualifier` annotation to specify which bean to inject when multiple candidates exist.

**Constructor Injection**: Injecting dependencies through constructor parameters. Most preferred method; enables immutability and explicit dependency declaration.

**Setter Injection**: Injecting dependencies through setter methods. Flexible but allows partial initialization and mutable objects.

**Field Injection**: Injecting dependencies directly into class fields using `@Autowired`. Convenient but less explicit and harder to test.

---

## Q&A

**Why is Dependency Injection important in Spring?**

DI decouples components, making code testable, maintainable, and flexible. Objects don't create their dependencies; Spring provides them. This enables easy mocking in unit tests, swapping implementations without code changes, and cleaner architecture following SOLID principles.

**What are the three types of dependency injection in Spring?**

Constructor injection passes dependencies through constructor parameters; setter injection uses setter methods; field injection annotates fields with `@Autowired`. Constructor injection is most recommended for mandatory dependencies and immutability. Setter injection for optional dependencies. Field injection discouraged in production due to testing difficulties and hidden dependencies.

**How does Spring resolve which bean to inject when multiple candidates exist?**

Spring first matches by type; if multiple beans of same type exist, it attempts to match by name (field/parameter name must match bean name). If still ambiguous, use `@Qualifier("beanName")` to explicitly specify which bean. `@Primary` annotation designates default bean when multiple exist.

**What's the difference between @Autowired and @Inject?**

Both perform autowiring; `@Autowired` is Spring-specific, `@Inject` is standard Java (JSR-330). `@Autowired` supports `required` attribute and Spring-specific features. `@Inject` is framework-agnostic, promoting portability across frameworks. Both work identically in Spring; choose `@Inject` for portability or `@Autowired` for Spring-specific features.

**Can you inject a bean into a static field?**

No, static fields cannot be autowired because they belong to class, not instances. Spring container manages instances; static members are class-level. Workaround: use `@Autowired` on non-static field, then expose via static method or use `ObjectFactory<T>` for lazy access.

**How do you inject optional dependencies?**

Use `Optional<T>` wrapper or `@Autowired(required=false)`. `Optional` is cleaner, allowing `optional.ifPresent(...)` for conditional processing. With `required=false`, null is injected if bean unavailable. Prefer `Optional` for clarity; `required=false` can hide configuration issues.

---

## Code Examples

**Example - Constructor Injection (Recommended):**
```java
@Service
public class UserService {
    private final UserRepository userRepository;
    private final EmailService emailService;
    
    // Constructor injection
    public UserService(UserRepository userRepository, EmailService emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }
    
    public void createUser(String email) {
        User user = userRepository.save(new User(email));
        emailService.sendWelcomeEmail(email);
    }
}
```

**Example - Setter Injection:**
```java
@Service
public class ProductService {
    private ProductRepository productRepository;
    
    @Autowired
    public void setProductRepository(ProductRepository repo) {
        this.productRepository = repo;
    }
}
```

**Example - Field Injection:**
```java
@Controller
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/users/{id}")
    public ResponseEntity<?> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
}
```

**Example - Qualifier with Multiple Beans:**
```java
@Configuration
public class DataSourceConfig {
    @Bean
    public DataSource primaryDB() { ... }
    
    @Bean
    public DataSource secondaryDB() { ... }
}

@Service
public class DatabaseService {
    private final DataSource primaryDB;
    private final DataSource secondaryDB;
    
    public DatabaseService(
        @Qualifier("primaryDB") DataSource primary,
        @Qualifier("secondaryDB") DataSource secondary) {
        this.primaryDB = primary;
        this.secondaryDB = secondary;
    }
}
```

**Example - Optional Dependencies:**
```java
@Service
public class NotificationService {
    private final Optional<SMSService> smsService;
    private final Optional<PushNotificationService> pushService;
    
    public NotificationService(
        Optional<SMSService> smsService,
        Optional<PushNotificationService> pushService) {
        this.smsService = smsService;
        this.pushService = pushService;
    }
    
    public void notify(String message) {
        smsService.ifPresent(service -> service.send(message));
        pushService.ifPresent(service -> service.send(message));
    }
}
```

---

## Key Points

- **Prefer Constructor Injection**: Enables immutability, explicit dependencies, easy testing via direct construction.
- **Avoid Field Injection in Production**: Hides dependencies, complicates testing (requires reflection), makes code less obvious.
- **Circular Dependency**: Constructor injection catches circular dependencies at startup; field injection hides them until runtime.
- **Immutability**: Constructor injection enables `final` fields; setter/field injection creates mutable objects.
- **Testing**: Constructor injection simplifies unit testing via direct instantiation without container.
