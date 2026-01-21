---
{"publish":true,"title":"Metrics","cssclasses":""}
---

## Definitions

**Micrometer**: Vendor-neutral metrics instrumentation library. Provides facade similar to SLF4J for logging. Supports multiple monitoring systems (Prometheus, Grafana, DataDog).

**Metrics**: Quantitative measurements tracking application behavior. Types: counters (incrementing values), gauges (current values), timers (duration measurements), distribution summaries.

**Counter**: Cumulative metric increasing over time. Tracks total requests, errors, events. Never decreases except on restart.

**Gauge**: Snapshot of current value at point in time. Tracks current memory usage, thread count, queue size. Can increase or decrease.

**Timer**: Records duration and rate of events. Measures request latency, method execution time. Provides mean, max, percentiles.

**Registry**: Central repository storing and managing metrics. MeterRegistry interface with implementations per monitoring system (PrometheusMeterRegistry, JmxMeterRegistry).

**Tags**: Key-value pairs adding dimensions to metrics. Enable filtering, grouping. Example: counter tagged with `method=GET`, `endpoint=/api/users`.

---

## Q&A

**Why collect metrics in production systems?**

Metrics provide quantitative insights into system health, performance, capacity. Alert on error rates >5%, detect slow requests (p95 >500ms), capacity planning (requests/sec growth). Proactive monitoring vs. reactive debugging. Business KPIs (orders/hour, conversion rate).

**Why use Micrometer for metrics?**

Micrometer provides vendor-neutral API; switch monitoring systems without code changes. Integrates seamlessly with Spring Boot Actuator, auto-configures many metrics (JVM, HTTP, database). Supports dimensional metrics (tags) for flexible querying. Standard in Spring Boot; widely adopted, well-documented.

**What metrics does Spring Boot provide automatically?**

Spring Boot auto-configures: JVM metrics (memory, GC, threads), HTTP metrics (request count, duration, status codes), database connection pool metrics, cache metrics, logging metrics. Exposed via Actuator `/actuator/metrics` endpoint. Enable specific registries (Prometheus) via dependencies.

**How do you create custom metrics?**

Inject `MeterRegistry`, create counters, timers, gauges. Example: `registry.counter("orders.created").increment()` or `Timer.Sample.start(registry)` for timing. Use `@Timed` annotation on methods for automatic timing. Tag metrics for filtering: `counter("requests", "endpoint", "/api/users")`.

**What's the difference between counter, gauge, and timer?**

- **Counter**: cumulative total (requests served). 
- **Gauge**: current state (active connections). 
- **Timer**: duration/rate (request latency). Counter for volume, gauge for capacity, timer for performance. All tagged for slicing (per endpoint, user type).

**How do you integrate Micrometer with Prometheus?**

Add `micrometer-registry-prometheus` dependency. Spring Boot autoconfigures PrometheusMeterRegistry. Metrics exposed via `/actuator/prometheus` endpoint in Prometheus format. Configure Prometheus scraping this endpoint periodically. Visualize in Grafana dashboards.

**What are metric tags and why use them?**

Tags are key-value pairs adding dimensions (labels) to metrics. Enable filtering, aggregation in queries. Example: tag requests by HTTP method, endpoint, status code. Query: "show error rate for POST /api/users". Without tags, need separate metric per combination. Tags reduce metric explosion.

---

## Code Examples

**Example - Custom Counter:**
```java
@Service
public class OrderService {
    private final MeterRegistry meterRegistry;
    private final Counter orderCounter;
    
    public OrderService(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        this.orderCounter = Counter.builder("orders.created")
            .description("Total orders created")
            .tag("service", "order-service")
            .register(meterRegistry);
    }
    
    public void createOrder(Order order) {
        // Business logic
        orderRepository.save(order);
        
        // Increment counter
        orderCounter.increment();
    }
}
```

**Example - Timer for Method Duration:**
```java
@Service
public class PaymentService {
    private final MeterRegistry meterRegistry;
    
    public PaymentService(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
    }
    
    public PaymentResponse processPayment(Payment payment) {
        Timer.Sample sample = Timer.start(meterRegistry);
        try {
            // Process payment
            PaymentResponse response = paymentGateway.charge(payment);
            
            sample.stop(Timer.builder("payment.processing.duration")
                .tag("status", "success")
                .register(meterRegistry));
            
            return response;
        } catch (Exception e) {
            sample.stop(Timer.builder("payment.processing.duration")
                .tag("status", "error")
                .register(meterRegistry));
            throw e;
        }
    }
}
```

**Example - @Timed Annotation:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping("/{id}")
    @Timed(value = "users.get", description = "Get user by ID")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}

// Enable @Timed
@Configuration
public class MetricsConfig {
    @Bean
    public TimedAspect timedAspect(MeterRegistry registry) {
        return new TimedAspect(registry);
    }
}
```

**Example - Gauge for Current Value:**
```java
@Component
public class QueueMonitor {
    private final Queue<Task> taskQueue;
    
    public QueueMonitor(MeterRegistry meterRegistry, Queue<Task> taskQueue) {
        this.taskQueue = taskQueue;
        
        Gauge.builder("queue.size", taskQueue, Queue::size)
            .description("Current task queue size")
            .tag("queue", "tasks")
            .register(meterRegistry);
    }
}
```

**Example - Prometheus Configuration:**
```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: prometheus,health,metrics
  metrics:
    export:
      prometheus:
        enabled: true
    tags:
      application: ${spring.application.name}
      environment: production

# pom.xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

**Example - Custom Metrics with Micrometer:**

```java
@Service
public class OrderService {
    private final Counter orderCounter;
    private final Timer orderTimer;
    
    public OrderService(MeterRegistry registry) {
        this.orderCounter = Counter.builder("orders.total")
            .description("Total orders processed")
            .tag("service", "order-service")
            .register(registry);
            
        this.orderTimer = Timer.builder("orders.processing.duration")
            .description("Order processing latency")
            .register(registry);
    }
    
    public Order createOrder(OrderRequest request) {
        Timer.Sample sample = Timer.start(MeterRegistry);
        
        try {
            // Business logic
            Order order = processOrder(request);
            orderCounter.increment();
            return order;
        } finally {
            sample.stop(orderTimer);
        }
    }
}

```

---

## Key Points

- **Dimensional Metrics**: Always tag metrics for flexible querying. Common tags: service name, environment, version.
- **Metric Naming**: Use dot notation (snake_case in Prometheus): `http.server.requests`, `jvm.memory.used`.
- **Cardinality**: Avoid high-cardinality tags (user ID, request ID). Causes metric explosion, performance issues.
- **Percentiles**: Configure percentiles (p50, p95, p99) for timers; understand latency distribution beyond averages.
- **Dashboard Tools**: Use Grafana for visualization; pre-built dashboards available for Spring Boot applications.
- **Four Golden Signals**: Latency, Traffic, Errors, Saturation. Monitor these first.    
- **SLOs**: Define service level objectives (p95 < 200ms). Alert on violations.
- **Business Metrics**: Track revenue KPIs alongside technical metrics.
