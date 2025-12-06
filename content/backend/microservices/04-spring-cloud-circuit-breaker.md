---
{"publish":true,"title":"Spring Cloud Circuit Breaker: Resilience & Fault Tolerance","cssclasses":""}
---

## Definitions

**Circuit Breaker**: Design pattern preventing cascade failures. When service fails repeatedly, circuit "opens," failing fast without calling failing service. Protects system resources.

**Resilience4j**: Lightweight fault tolerance library for Java 8+. Provides circuit breaker, rate limiter, retry, bulkhead, time limiter. Modern alternative to Hystrix (deprecated).

**Circuit States**: Closed (normal operation, requests pass through), Open (service failing, requests fail immediately), Half-Open (testing if service recovered, limited requests allowed).

**Fallback**: Alternative response when circuit open or request fails. Provides degraded functionality maintaining system availability.

**Bulkhead**: Isolates resources (threads, connections) for different services. Prevents one failing service consuming all resources, starving others.

**Retry**: Automatically retrying failed requests. Configurable attempts, delays, exponential backoff. Useful for transient failures.

**Rate Limiter**: Limits requests per time period. Protects service from overload, ensures fair resource usage.

---

## Q&A

**How does circuit breaker pattern work?**

Circuit starts Closed (requests pass through). If failure rate exceeds threshold (e.g., 50% of 10 requests), circuit Opens (fails fast, no calls to service). After timeout (e.g., 60 seconds), circuit enters Half-Open (limited test requests). If tests succeed, circuit Closes; if fail, reopens. Prevents overloading failing services, enables graceful degradation.

**When should you use circuit breakers?**

Use when calling external services, databases, or other microservices where failures possible. Prevents cascade failures (one service failure bringing down entire system). Essential in microservices; single slow/failing service shouldn't crash others. Combine with timeouts, retries for comprehensive resilience.

**What's the difference between circuit breaker and retry?**

Retry immediately retries failed requests (configurable attempts, delays). Circuit breaker stops calling failing service after threshold reached. Retry handles transient failures (network blips); circuit breaker handles sustained failures (service down). Use together: retry for transient failures, circuit breaker for persistent failures.

**How do you implement fallback methods?**

Define fallback method with same signature as original. Annotate original with `@CircuitBreaker(name="...", fallbackMethod="...")`. When circuit open or exception occurs, fallback executes. Fallback returns cached data, default value, or error message. Enables degraded functionality vs. complete failure.

**What metrics does Resilience4j provide?**

Tracks failure rate, slow call rate, call duration, circuit state, buffer sizes. Exposed via actuator endpoints for monitoring. Integrate with Prometheus, Grafana for visualization. Helps identify problematic services, tune circuit breaker configuration.

**How do you configure circuit breaker thresholds?**

Configure failure rate threshold (percentage), slow call rate threshold, slow call duration, wait duration in open state, permitted calls in half-open, sliding window size. Tune based on service SLAs and acceptable failure rates. Too sensitive causes unnecessary opening; too lenient allows prolonged failures.

---

## Code Examples

**Example - Circuit Breaker Configuration:**
```java
@Configuration
public class Resilience4jConfig {
    @Bean
    public CircuitBreakerConfig circuitBreakerConfig() {
        return CircuitBreakerConfig.custom()
            .failureRateThreshold(50)  // Open if 50% fail
            .waitDurationInOpenState(Duration.ofMillis(60000))  // 60s
            .permittedNumberOfCallsInHalfOpenState(3)
            .slidingWindowSize(10)  // Last 10 calls
            .build();
    }
}

// application.yml
resilience4j:
  circuitbreaker:
    instances:
      userService:
        failure-rate-threshold: 50
        wait-duration-in-open-state: 60000ms
        sliding-window-size: 10
        minimum-number-of-calls: 5
```

**Example - Service with Circuit Breaker:**
```java
@Service
public class OrderService {
    @Autowired
    private RestTemplate restTemplate;
    
    @CircuitBreaker(name = "userService", fallbackMethod = "getUserFallback")
    public User getUser(Long userId) {
        String url = "http://user-service/api/users/" + userId;
        return restTemplate.getForObject(url, User.class);
    }
    
    // Fallback method - same signature plus Throwable
    private User getUserFallback(Long userId, Throwable t) {
        // Return cached or default user
        return new User(userId, "Unknown User", "unknown@example.com");
    }
}
```

**Example - Retry Configuration:**
```java
@Service
public class PaymentService {
    
    @Retry(name = "paymentService", fallbackMethod = "processPaymentFallback")
    public PaymentResponse processPayment(Payment payment) {
        // May fail transiently; retry automatically
        return paymentGateway.charge(payment);
    }
    
    private PaymentResponse processPaymentFallback(Payment payment, Exception e) {
        return new PaymentResponse("FAILED", "Payment service unavailable");
    }
}

// application.yml
resilience4j:
  retry:
    instances:
      paymentService:
        max-attempts: 3
        wait-duration: 1000ms
        exponential-backoff-multiplier: 2
```

**Example - Bulkhead Configuration:**
```java
@Service
public class NotificationService {
    
    @Bulkhead(name = "notificationService", type = Bulkhead.Type.SEMAPHORE)
    public void sendNotification(String message) {
        // Max concurrent calls limited by bulkhead
        emailService.send(message);
    }
}

// application.yml
resilience4j:
  bulkhead:
    instances:
      notificationService:
        max-concurrent-calls: 10
        max-wait-duration: 500ms
```

**Example - Combining Patterns:**
```java
@Service
public class ProductService {
    
    @CircuitBreaker(name = "productService")
    @Retry(name = "productService")
    @RateLimiter(name = "productService")
    public Product getProduct(Long id) {
        return externalProductApi.fetchProduct(id);
    }
}
```

---

## Key Points

- **Fail Fast**: Circuit breaker fails immediately when open; saves resources vs. waiting for timeout.
- **Monitoring**: Monitor circuit state, failure rates via actuator. Set up alerts for frequently opening circuits.
- **Testing**: Test fallback methods thoroughly; they're executed during failures, must work correctly.
- **Configuration Tuning**: Start with conservative thresholds; tune based on production metrics and SLAs.
- **Timeouts**: Always combine circuit breakers with timeouts; prevents hanging on slow services.
