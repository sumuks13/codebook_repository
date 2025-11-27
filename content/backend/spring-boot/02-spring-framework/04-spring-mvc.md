---
{"publish":true,"title":"Spring MVC","cssclasses":""}
---

## Definitions

**Spring MVC**: Web framework implementing Model-View-Controller pattern. Provides components for request handling, view rendering, and model management.

**DispatcherServlet**: Central servlet routing all HTTP requests to appropriate handlers. Coordinates request processing, exception handling, and view resolution.

**Controller**: Component handling HTTP requests, executing business logic, returning ModelAndView or direct response. Mapped to URLs via `@RequestMapping` or shortcuts.

**Model**: Data passed from controller to view for rendering. Contains request attributes, model attributes populated via `@ModelAttribute`, `model.addAttribute()`.

**View**: Renders response to client. Common implementations: JSP, Thymeleaf, Freemarker, JSON/XML representations. View resolver maps logical view names to actual views.

**Handler Mapping**: Component determining which controller handles incoming request based on URL pattern. Implementations: RequestMappingHandlerMapping (annotation-based), SimpleUrlHandlerMapping (URL pattern-based).

---

## Q&A

**How does Spring MVC handle HTTP requests?**

DispatcherServlet receives request, queries handler mappings to find appropriate controller, invokes controller method, receives ModelAndView, queries view resolver to find view implementation, renders view with model data, returns response to client. Exception handlers catch exceptions, applying consistent error handling. This pipeline standardizes request processing across application.

**What's the difference between @RequestMapping and route-specific annotations?**

`@RequestMapping` is generic, mapping to HTTP method, path, headers, parameters. Route-specific annotations (`@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`) are shortcuts for specific HTTP methods. `@GetMapping("/users")` equivalent to `@RequestMapping(value="/users", method=RequestMethod.GET)`. Shortcuts cleaner for typical REST endpoints; `@RequestMapping` used when needing non-standard HTTP method or complex conditions.

**How do you pass data from controller to view?**

Use Model/ModelMap parameter in controller method: `model.addAttribute("users", userList)`. Alternatively, return ModelAndView specifying view name and model. Spring automatically exposes request attributes to view template. In JSP: `${users}` accesses model attribute. In Thymeleaf: `th:each="user : ${users}"` iterates collection.

**What are request and response scopes in Spring MVC?**

Request scope: data specific to single HTTP request; lost after response sent. Response scope: not directly supported; use redirectAttributes for POST-redirect-GET pattern. Session scope: persists across multiple requests from same user. Application scope: shared across all sessions/users. Choose scope based on data lifetime requirements.

**How do you handle exceptions in Spring MVC?**

Use `@ExceptionHandler` methods in controller or `@ControllerAdvice` global exception handler. Methods catch specific exceptions, return appropriate response (ModelAndView, JSON, HTTP status). `@ExceptionHandler(SQLException.class)` catches SQLException, allowing custom error page or JSON error response. Global handlers via `@ControllerAdvice` apply to all controllers.

**How do you validate user input in Spring MVC?**

Use JSR-303/JSR-380 validation annotations (`@NotNull`, `@NotBlank`, `@Email`, `@Min`, `@Max`) on model properties. In controller, add `@Valid` annotation to parameter, then check BindingResult for errors. Custom validators implement `Validator` interface. Spring automatically validates and populates BindingResult with validation errors for error display.

---

## Code Examples

**Example - Simple Controller with RequestMapping:**
```java
@Controller
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping
    public String listUsers(Model model) {
        List<User> users = userService.getAllUsers();
        model.addAttribute("users", users);
        return "users/list";  // View name
    }
    
    @GetMapping("/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        User user = userService.getUserById(id);
        model.addAttribute("user", user);
        return "users/detail";
    }
    
    @PostMapping
    public String createUser(@Valid @ModelAttribute User user, 
                            BindingResult result) {
        if (result.hasErrors()) {
            return "users/form";
        }
        userService.saveUser(user);
        return "redirect:/users";
    }
}
```

**Example - Global Exception Handler:**
```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<?> handleNotFound(ResourceNotFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(new ErrorResponse("Resource not found", e.getMessage()));
    }
    
    @ExceptionHandler(SQLException.class)
    public ResponseEntity<?> handleDatabaseError(SQLException e) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(new ErrorResponse("Database error", "Please try again"));
    }
}
```

**Example - Model Validation:**
```java
@Data
@Entity
public class User {
    @Id
    @GeneratedValue
    private Long id;
    
    @NotBlank(message = "Name is required")
    private String name;
    
    @Email(message = "Email should be valid")
    private String email;
    
    @Min(value = 18, message = "Age must be at least 18")
    private int age;
}

@PostMapping("/users")
public String saveUser(@Valid User user, BindingResult result) {
    if (result.hasErrors()) {
        return "users/form";  // Redisplay form with errors
    }
    userService.save(user);
    return "redirect:/users";
}
```

**Example - REST Controller with JSON:**
```java
@RestController
@RequestMapping("/api/users")
public class UserRestController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody @Valid User user) {
        User saved = userService.saveUser(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(saved);
    }
}
```

---

## Key Points

- **DispatcherServlet Configuration**: Typically configured in web.xml or via Spring Boot autoconfiguration; most developers never manually configure it.
- **View Resolution**: Configure view resolver for your template engine; multiple resolvers chained for fallback support.
- **RESTful Design**: Use HTTP methods (GET/POST/PUT/DELETE) appropriately; `@RestController` automatically serializes responses to JSON.
- **Validation Order**: Validate at multiple layers (client-side, controller input, business logic, database constraints) for robustness.
- **Exception Hierarchy**: Define specific exception handlers for expected errors; general handlers catch unexpected errors gracefully.
