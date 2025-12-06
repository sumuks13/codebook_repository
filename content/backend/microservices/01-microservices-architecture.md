---
{"publish":true,"title":"Microservices Architecture","cssclasses":""}
---

## Definitions

**Microservices**: Architectural style structuring application as collection of small, independent services. Each service implements specific business capability, deployable independently.

**Service Autonomy**: Each microservice owns its data, logic, deployment. Can be developed, deployed, scaled independently without affecting others.

**API Gateway**: Entry point for clients. Routes requests to appropriate microservices, handles cross-cutting concerns (authentication, rate limiting, logging).

**Service Discovery**: Mechanism enabling services to find each other dynamically. Registry maintains service instances; clients query registry for service locations.

**Inter-Service Communication**: Services communicate via REST APIs, message queues, gRPC. Synchronous (REST, gRPC) or asynchronous (messaging, events).

**Circuit Breaker**: Pattern preventing cascade failures. When service fails repeatedly, circuit opens, failing fast without calling failing service. Protects system from overload.

**Distributed Transaction**: Transaction spanning multiple services. Challenging in microservices; prefer eventual consistency patterns (Saga, CQRS) over distributed locks.

---

## Q&A

**Why use microservices instead of monolithic architecture?**

Microservices enable independent deployment (faster releases), technology diversity (choose best tool per service), fault isolation (one service failure doesn't crash system), scalability (scale individual services based on demand). Trade-offs: increased complexity, network latency, distributed transactions challenges. Choose microservices when team size, application complexity justify overhead.

**What are the challenges of microservices architecture?**

Challenges include: distributed system complexity, network reliability issues, data consistency across services, debugging/tracing across services, deployment coordination, testing complexity. Solutions: service mesh, distributed tracing, API gateways, circuit breakers, well-defined contracts. Start with monolith; migrate to microservices when benefits outweigh costs.

**How do you handle data consistency in microservices?**

Avoid distributed transactions; use eventual consistency patterns. Saga pattern coordinates transactions across services using compensating transactions. Event sourcing stores state changes as events; services subscribe, update locally. CQRS separates read/write models for flexibility. Accept temporary inconsistency; design for reconciliation.

**What communication patterns exist between microservices?**

Synchronous: REST APIs (simple, widely supported), gRPC (fast, binary protocol). Asynchronous: message queues (RabbitMQ, Kafka), event-driven (publish/subscribe). Synchronous simpler but couples services; asynchronous decouples, improves resilience. Choose based on use case: queries synchronous, commands/events asynchronous.

**How do you handle service failures in microservices?**

Implement circuit breakers (fail fast when service down), retries with exponential backoff, timeouts (prevent hanging), fallback mechanisms (default responses), bulkheads (isolate resources). Monitor health endpoints. Design for failure; assume services occasionally unavailable.

**What is the role of API Gateway in microservices?**

API Gateway provides single entry point for clients, routing to appropriate services. Handles authentication, rate limiting, request/response transformation, protocol translation (REST to gRPC). Simplifies client logic; centralizes cross-cutting concerns. Examples: Spring Cloud Gateway, Netflix Zuul.

---

## Code Examples

**Example - Microservice Structure:**
```java
// User Service
@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
}

// Order Service (separate microservice)
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    @Autowired
    private OrderService orderService;
    
    @Autowired
    private RestTemplate restTemplate;
    
    @GetMapping("/{id}")
    public ResponseEntity<OrderDTO> getOrder(@PathVariable Long id) {
        Order order = orderService.getOrderById(id);
        
        // Call User Service
        String userServiceUrl = "http://user-service/api/users/" + order.getUserId();
        User user = restTemplate.getForObject(userServiceUrl, User.class);
        
        return ResponseEntity.ok(new OrderDTO(order, user));
    }
}
```

**Example - Service Communication with RestTemplate:**
```java
@Configuration
public class RestTemplateConfig {
    @Bean
    @LoadBalanced  // Enables service discovery
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

@Service
public class OrderService {
    @Autowired
    private RestTemplate restTemplate;
    
    public OrderWithUserDTO getOrderWithUser(Long orderId) {
        Order order = orderRepository.findById(orderId).get();
        
        // Synchronous call to User Service
        User user = restTemplate.getForObject(
            "http://user-service/api/users/" + order.getUserId(),
            User.class
        );
        
        return new OrderWithUserDTO(order, user);
    }
}
```

**Example - Saga Pattern (Choreography):**
```java
// Order Service - publishes event
@Service
public class OrderService {
    @Autowired
    private KafkaTemplate<String, OrderCreatedEvent> kafkaTemplate;
    
    @Transactional
    public Order createOrder(Order order) {
        Order saved = orderRepository.save(order);
        
        // Publish event
        OrderCreatedEvent event = new OrderCreatedEvent(
            saved.getId(), saved.getUserId(), saved.getTotalAmount()
        );
        kafkaTemplate.send("order-created", event);
        
        return saved;
    }
}

// Payment Service - subscribes to event
@Service
public class PaymentEventListener {
    @KafkaListener(topics = "order-created")
    public void handleOrderCreated(OrderCreatedEvent event) {
        try {
            paymentService.processPayment(event.getUserId(), event.getAmount());
            // Publish success event
        } catch (PaymentException e) {
            // Publish failure event; Order Service compensates
        }
    }
}
```

---

## Key Points

- **Start with Monolith**: Begin simple; migrate to microservices when complexity justifies. Premature microservices adds unnecessary overhead.
- **Domain-Driven Design**: Define service boundaries based on business capabilities (bounded contexts). Poor boundaries cause tight coupling.
- **Database per Service**: Each service owns its database; no shared database. Enforces loose coupling but complicates queries spanning services.
- **Observability**: Implement distributed tracing (Zipkin, Jaeger), centralized logging (ELK), metrics (Prometheus). Essential for debugging.
- **DevOps Culture**: Microservices require mature DevOps: CI/CD pipelines, containerization (Docker), orchestration (Kubernetes).
