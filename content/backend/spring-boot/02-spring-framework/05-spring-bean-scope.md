---
{"publish":true,"title":"Bean Scope","cssclasses":""}
---

## Definitions

**Bean Scope**: Lifecycle and visibility of bean instance within Spring container. Determines how many instances created and how long they persist.

**Singleton Scope**: Single bean instance created per IoC container; shared across entire application. Default scope; all requests receive same instance.

**Prototype Scope**: New bean instance created every time requested. Each call to `getBean()` returns new instance. Useful for stateful objects.

**Request Scope**: New bean instance created for each HTTP request. Instance available throughout request lifecycle. Web applications only.

**Session Scope**: Single bean instance per user session. Persists across multiple requests from same user. Web applications only.

**Application Scope**: Single bean instance per ServletContext. Shared across all sessions and users. Web applications only.

---

## Q&A

**Why is singleton the default scope?**

Singleton minimizes memory usage (one instance serves all requests), improves performance (no repeated instantiation overhead), and is sufficient for stateless beans (services, repositories). Most beans are stateless and benefit from singleton efficiency. Prototype used only when statefulness requires isolation.

**When should you use prototype scope?**

Use prototype for stateful beans where each request needs isolated state. Examples: form command objects, request-scoped data holders, mutable objects shared between threads. Never use prototype for expensive-to-create beans without profiling impact first.

**What's the difference between request and session scope?**

Request scope creates new bean per HTTP request; disposed after response sent. Session scope creates bean per user session; persists across multiple requests from same user until session expires. Request scope for request-specific data; session scope for user profile, shopping cart, preferences.

**How do you use request/session scoped beans in singleton services?**

Can't directly inject request/session beans into singleton beans; singleton instantiated once, request/session beans vary. Solutions: (1) use `@Scope(proxyMode = ScopedProxyMode.TARGET_CLASS)` creating proxy forwarding to actual bean, (2) inject `ObjectFactory<Bean>` calling `getObject()` when needed, (3) use `@Lookup` method annotation for lazy retrieval.

**Is singleton scope thread-safe?**

Singleton scope itself doesn't guarantee thread-safety; only means one instance shared. Thread-safety depends on bean implementation. Stateless singleton beans (no mutable fields) thread-safe; stateful singletons require synchronization. This is critical distinction: singleton scope ≠ thread-safe implementation.

**Can you change scope at runtime?**

No, scope determined at bean definition time and fixed for bean lifetime. Cannot dynamically switch instance between singleton and prototype. Must redefine bean with different scope, requiring container restart.

---

## Code Examples

**Example - Singleton Scope (Default):**
```java
@Component
@Scope("singleton")  // Default, can omit
public class SingletonService {
    private int counter = 0;
    
    public void increment() {
        counter++;
        System.out.println("Counter: " + counter);
    }
}

// Usage: Every call receives same instance
SingletonService s1 = context.getBean(SingletonService.class);
SingletonService s2 = context.getBean(SingletonService.class);
System.out.println(s1 == s2);  // true
```

**Example - Prototype Scope:**
```java
@Component
@Scope("prototype")
public class PrototypeService {
    private String id = UUID.randomUUID().toString();
    
    public String getId() {
        return id;
    }
}

// Usage: Each call creates new instance
PrototypeService p1 = context.getBean(PrototypeService.class);
PrototypeService p2 = context.getBean(PrototypeService.class);
System.out.println(p1 == p2);  // false
```

**Example - Request Scope with Proxy:**
```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestData {
    private String userId;
    
    public void setUserId(String userId) {
        this.userId = userId;
    }
    
    public String getUserId() {
        return userId;
    }
}

@Service
public class MyService {
    @Autowired
    private RequestData requestData;  // Proxy injected
    
    public void process() {
        String userId = requestData.getUserId();
    }
}
```

**Example - Session Scope:**
```java
@Component
@Scope("session")
@Data
public class UserSession {
    private String username;
    private List<String> permissions;
    private LocalDateTime loginTime;
}

@Controller
public class LoginController {
    @Autowired
    private UserSession userSession;  // Proxy injected
    
    @PostMapping("/login")
    public String login(String username, String password) {
        // Validate credentials
        userSession.setUsername(username);
        userSession.setPermissions(loadPermissions(username));
        userSession.setLoginTime(LocalDateTime.now());
        return "redirect:/home";
    }
}
```

**Example - ObjectFactory for Lazy Lookup:**
```java
@Service
public class ServiceWithPrototype {
    @Autowired
    private ObjectFactory<PrototypeService> serviceFactory;
    
    public void process() {
        PrototypeService service = serviceFactory.getObject();
        // Use service
    }
}
```

---

## Key Points

- **Default Singleton**: Most beans are singleton; remember to design stateless if using singleton scope.
- **Web Scope Limitations**: Request/session scopes only work in web applications; fail silently in non-web contexts.
- **Memory Implications**: Singleton beans persist for application lifetime; large singletons consume memory. Prototype creates many instances; balance based on frequency.
- **Thread-Safety Responsibility**: Singleton scope doesn't provide thread-safety; developers must ensure bean implementation is thread-safe.
- **Scope Proxying**: Proxies add slight overhead; use only when necessary for cross-scope injection.
