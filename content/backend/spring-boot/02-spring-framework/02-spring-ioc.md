---
{"publish":true,"title":"IoC (Inversion of Control)","cssclasses":""}
---

## Definitions

**Inversion of Control (IoC)**: Design principle where control flow is inverted; framework controls object creation and lifecycle instead of application code. Spring's IoC container manages bean instantiation, initialization, and dependency resolution.

**IoC Container**: Spring component managing bean lifecycle from creation through destruction. Reads bean definitions (XML, annotations, Java config), creates instances, injects dependencies, manages scope and initialization.

**Bean Lifecycle**: Sequence of phases a Spring bean goes through: instantiation, population of properties, calling initialization methods, use, and finally destruction. Can hook into phases via callbacks.

**ApplicationContext**: Main IoC container interface providing bean factory capabilities plus additional features (event publishing, message resources). Implementations: ClassPathXmlApplicationContext, AnnotationConfigApplicationContext.

**BeanFactory**: Core interface for accessing beans from container. ApplicationContext extends BeanFactory adding resource loading, event publishing, and message resolution.

**Bean Definition**: Metadata describing how to create a bean: class, constructor arguments, property values, initialization/destruction methods. Defined via XML, annotations, or Java configuration.

---

## Q&A

**How does Spring's IoC container work?**

IoC container reads bean definitions (from XML, annotations, or Java config), reflects on classes to understand dependencies, instantiates beans, injects dependencies via constructor/setter/field, calls initialization methods, and manages lifecycle until shutdown. Container maintains bean registry, handles scope management (singleton/prototype/request/session), and coordinates all bean interactions.

**What is the difference between BeanFactory and ApplicationContext?**

BeanFactory is minimal interface for bean access and lifecycle management. ApplicationContext extends BeanFactory adding resource loading (loading configuration files from classpath, URLs), internationalization (message resources), and event publishing. ApplicationContext is eager (loads all singletons on startup); BeanFactory is lazy (loads on first access). ApplicationContext preferred for enterprise applications.

**What is the bean lifecycle in Spring?**

Bean lifecycle: (1) bean definition metadata read, (2) bean instantiated via constructor, (3) dependencies injected, (4) BeanNameAware/BeanFactoryAware/ApplicationContextAware methods called if bean implements interfaces, (5) BeanPostProcessor pre-initialization called, (6) @PostConstruct or init-method called, (7) BeanPostProcessor post-initialization called, (8) bean ready for use, (9) on shutdown, @PreDestroy or destroy-method called, then bean discarded.

**How do you customize bean initialization and destruction?**

Three ways: (1) implement InitializingBean/DisposableBean interfaces (not recommended), (2) use `@PostConstruct` and `@PreDestroy` annotations (recommended), (3) specify `init-method` and `destroy-method` in bean definition. `@PostConstruct` method called after dependency injection; `@PreDestroy` called before bean destruction. These allow custom setup (database connections, resource pools) and cleanup.

**What is the difference between singleton and prototype scope?**

Singleton (default) creates one bean instance per IoC container, shared across entire application. Thread-safe by design; all requests receive same instance. Prototype creates new instance every time bean is requested. Useful for stateful beans or when data isolation needed. Singleton scope reduces memory; prototype increases isolation but with overhead.

**How do you programmatically retrieve beans from container?**

Use ApplicationContext reference: `applicationContext.getBean("beanName")` retrieves by name, `applicationContext.getBean(BeanClass.class)` retrieves by type, `applicationContext.getBean(BeanClass.class, "beanName")` retrieves by type and name. Also supports lambda expressions for conditional retrieval: `getBeansOfType(Class<T>)` returns all beans of type.

---

## Code Examples

**Example - Bean Lifecycle with Callbacks:**
```java
@Component
public class DataSource {
    private Connection connection;
    
    @PostConstruct
    public void initialize() {
        System.out.println("Initializing DataSource");
        connection = createConnection();
    }
    
    @PreDestroy
    public void cleanup() {
        System.out.println("Cleaning up DataSource");
        if (connection != null) {
            connection.close();
        }
    }
    
    private Connection createConnection() {
        // Create database connection
        return null;
    }
}
```

**Example - Java Configuration (IoC Container Setup):**
```java
@Configuration
public class AppConfig {
    @Bean
    public UserRepository userRepository() {
        return new UserRepositoryImpl();
    }
    
    @Bean
    public UserService userService(UserRepository repo) {
        return new UserService(repo);
    }
}

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = 
            new AnnotationConfigApplicationContext(AppConfig.class);
        UserService userService = context.getBean(UserService.class);
        userService.createUser("user@example.com");
    }
}
```

**Example - InitializingBean/DisposableBean (Not Recommended):**
```java
@Component
public class LegacyService implements InitializingBean, DisposableBean {
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("Initializing LegacyService");
    }
    
    @Override
    public void destroy() throws Exception {
        System.out.println("Destroying LegacyService");
    }
}
```

**Example - Programmatic Bean Retrieval:**
```java
@Component
public class BeanAccessor {
    @Autowired
    private ApplicationContext context;
    
    public void accessBeans() {
        // By name
        UserService service = 
            (UserService) context.getBean("userService");
        
        // By type
        UserService service2 = context.getBean(UserService.class);
        
        // Get all beans of type
        Map<String, UserRepository> repos = 
            context.getBeansOfType(UserRepository.class);
    }
}
```

---

## Key Points

- **Container Instantiation**: Spring creates all singleton beans on ApplicationContext initialization (eager loading); prototype beans created on demand.
- **Circular Dependencies**: Constructor injection catches circular dependencies; field/setter injection defers detection to runtime.
- **Resource Management**: Always implement cleanup in `@PreDestroy` for proper resource release (database connections, file handles, thread pools).
- **Scope Considerations**: Singleton beans are thread-safe only if stateless; stateful beans should use prototype or request scope.
- **Performance**: Singleton scope reduces overhead but may cause memory issues with large objects; profile and measure impact.
