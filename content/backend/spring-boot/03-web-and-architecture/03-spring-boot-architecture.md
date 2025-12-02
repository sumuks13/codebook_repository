---
{"publish":true,"title":"Spring Boot Architecture","cssclasses":""}
---

## Definitions

**Layered Architecture**: Organizing application into logical layers (Controller, Service, Repository, Model). Each layer has specific responsibility; layers communicate through interfaces.

**Controller Layer**: Handles HTTP requests, orchestrates business logic, returns responses. Maps URLs to methods via @RequestMapping.

**Service Layer**: Contains business logic, transactions, validations. Coordinates between controller and repository layers.

**Repository Layer (DAO)**: Data access abstraction layer. Encapsulates database operations (CRUD, queries). Enables switching databases without business logic changes.

**Model/Entity**: Domain objects representing business concepts. Annotated with JPA @Entity for ORM mapping.

**DTO (Data Transfer Object)**: Simplified object transferring data between layers. Contains only necessary fields, reducing over-fetching and coupling.

**Spring Bean**: Object managed by Spring IoC container. Created, configured, and disposed by container throughout application lifecycle.

---

## Q&A

**Why layer applications?**

Layered architecture provides separation of concerns: each layer has single responsibility. Enables unit testing in isolation, easy maintenance, clear code organization. Changes in one layer don't affect others if interfaces well-defined. Standard practice across enterprise applications.

**What's the difference between DTO and Entity?**

Entity represents database table structure; DTO represents data for transfer. Entity contains all columns; DTO contains only needed fields. Entity tied to database; DTO decouples API from database schema. Use DTOs in REST APIs, use Entities for database operations. Prevents over-fetching, provides API stability when database schema changes.

**How do you handle cross-layer communication?**

Use dependency injection; each layer injects lower layer interfaces. Controller injects Service interface, Service injects Repository interface. Layers call methods on injected dependencies. Never call upper layer from lower layer (would create circular dependency).

**What's the role of Service layer?**

Service implements business logic, transactions, validations, orchestration. Coordinates between Controller (incoming requests) and Repository (data access). Multiple controllers can use same service for code reuse. Centralizes business rules in one place for consistency.

**How do you prevent tight coupling between layers?**

Use interfaces; Controller depends on Service interface, not implementation. Service depends on Repository interface. Enables mocking in tests, swapping implementations, reducing dependencies. Invert dependencies: always depend on abstractions, not concrete classes.

**What's the typical Spring Boot project structure?**

Standard structure: `src/main/java/` contains code organized by layer (controller, service, repository, model packages), `src/main/resources/` contains application.properties/yml and templates, `src/test/java/` contains tests, `pom.xml`/`build.gradle` manages dependencies. Follow this structure for consistency across projects.

---

## Code Examples

**Example - Layered Architecture:**
```java
// Model/Entity Layer
@Entity
@Table(name = "users")
@Data
public class User {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
    private String email;
}

// DTO
@Data
@AllArgsConstructor
public class UserDTO {
    private Long id;
    private String name;
    private String email;
}

// Repository Layer
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}

// Service Layer
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public UserDTO getUserById(Long id) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
        return convertToDTO(user);
    }
    
    private UserDTO convertToDTO(User user) {
        return new UserDTO(user.getId(), user.getName(), user.getEmail());
    }
}

// Controller Layer
@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
        UserDTO user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}
```

**Example - Dependency Injection Across Layers:**
```java
@RestController
public class OrderController {
    private final OrderService orderService;
    
    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }
    
    @PostMapping("/orders")
    public ResponseEntity<?> createOrder(@RequestBody Order order) {
        return ResponseEntity.ok(orderService.createOrder(order));
    }
}

@Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    
    public OrderService(OrderRepository repo, PaymentService payment) {
        this.orderRepository = repo;
        this.paymentService = payment;
    }
    
    public Order createOrder(Order order) {
        paymentService.processPayment(order.getAmount());
        return orderRepository.save(order);
    }
}
```

---

## Key Points

- **N-Tier Architecture**: Common pattern using 3-4 layers (Controller, Service, Repository, Model). Each layer independent; communication through interfaces.
- **DTO Pattern**: Always use DTOs for REST APIs, never expose Entities directly. Prevents schema leakage, enables evolution, improves security.
- **Transaction Boundaries**: Place `@Transactional` on Service methods; typically not on Repositories or Controllers. Ensures data consistency across database operations.
- **Testing Strategy**: Test each layer separately; mock dependencies. Controller tests mock Service, Service tests mock Repository.
- **SOLID Principles**: Layered architecture supports Single Responsibility (each layer one job), Dependency Inversion (depend on abstractions), Open/Closed (easy to extend layers).
