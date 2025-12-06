---
{"publish":true,"title":"Spring Cloud OpenFeign: Declarative REST Client","cssclasses":""}
---

## Definitions

**OpenFeign**: Declarative REST client simplifying HTTP API calls. Define interface; Feign generates implementation automatically.

**@FeignClient**: Annotation marking interface as Feign client. Specifies service name (for Eureka) or URL. Spring creates proxy implementing interface.

**Request Mapping**: Use Spring MVC annotations (@GetMapping, @PostMapping) defining HTTP endpoints. Feign translates to HTTP requests.

**Load Balancing**: When integrated with Eureka, Feign automatically load balances across service instances. Client-side load balancing via Spring Cloud LoadBalancer.

**Fallback**: Fallback class implementing same interface. Executed when service call fails or circuit breaker opens. Provides graceful degradation.

**Request Interceptor**: Component intercepting requests before sending. Adds headers (authentication tokens), modifies requests, implements cross-cutting concerns.

**Error Decoder**: Custom error handling translating HTTP error codes to exceptions. Enables consistent error handling across Feign clients.

---

## Q&A

**Why use Feign instead of RestTemplate?**

Feign provides declarative, annotation-based API; cleaner, less boilerplate than RestTemplate. Integrates seamlessly with Eureka (service discovery), Ribbon/LoadBalancer (load balancing), Resilience4j (circuit breakers). RestTemplate requires manual URL construction, error handling, retries. Feign generates implementation automatically.

**How does Feign integrate with Eureka?**

Specify service name in `@FeignClient(name="user-service")`. Feign queries Eureka for available instances, load balances automatically. No hardcoded URLs; dynamic discovery. If service scaled up/down, Feign adapts automatically. Combines service discovery with REST client simplicity.

**How do you handle errors in Feign clients?**

Implement custom ErrorDecoder translating HTTP status codes to exceptions. Or use fallback classes with `@FeignClient(fallback=...)`. Circuit breakers automatically integrated when Resilience4j present. Combine approaches: ErrorDecoder for specific exceptions, fallback for graceful degradation.

**Can you pass headers and query parameters with Feign?**

Yes, use `@RequestHeader` for headers, `@RequestParam` for query params, `@PathVariable` for path variables. Example: `User getUser(@PathVariable Long id, @RequestHeader("Authorization") String token)`. Feign automatically includes parameters in HTTP request.

**How do you configure timeouts in Feign?**

Configure connection and read timeouts in `application.yml` under `feign.client.config`. Set per-client or default. Example: `feign.client.config.user-service.connectTimeout=5000` and `readTimeout=10000`. Important for preventing hanging on slow services.

**What's the difference between Feign and WebClient?**

Feign is declarative (interface-based), blocking by default. WebClient reactive (Flux/Mono), non-blocking. Feign simpler for traditional REST APIs; WebClient better for reactive applications, streaming, high concurrency. Feign integrated with Spring Cloud ecosystem; WebClient more modern, flexible.

---

## Code Examples

**Example - Basic Feign Client:**
```java
@FeignClient(name = "user-service")
public interface UserClient {
    
    @GetMapping("/api/users/{id}")
    User getUserById(@PathVariable Long id);
    
    @PostMapping("/api/users")
    User createUser(@RequestBody User user);
    
    @GetMapping("/api/users")
    List<User> getAllUsers(@RequestParam String status);
}

// Usage
@Service
public class OrderService {
    @Autowired
    private UserClient userClient;
    
    public void processOrder(Long userId) {
        User user = userClient.getUserById(userId);
        // Process order
    }
}
```

**Example - Feign with Fallback:**
```java
@FeignClient(name = "user-service", fallback = UserClientFallback.class)
public interface UserClient {
    @GetMapping("/api/users/{id}")
    User getUserById(@PathVariable Long id);
}

@Component
public class UserClientFallback implements UserClient {
    @Override
    public User getUserById(Long id) {
        return new User(id, "Unknown", "unknown@example.com");
    }
}
```

**Example - Configuration:**
```yaml
# application.yml
feign:
  client:
    config:
      default:
        connectTimeout: 5000
        readTimeout: 10000
        loggerLevel: full
      user-service:
        connectTimeout: 3000
        readTimeout: 5000

# Enable Feign
spring:
  cloud:
    openfeign:
      circuitbreaker:
        enabled: true
```

**Example - Request Interceptor:**
```java
@Configuration
public class FeignConfig {
    
    @Bean
    public RequestInterceptor requestInterceptor() {
        return template -> {
            // Add authentication token
            String token = SecurityContextHolder.getContext()
                .getAuthentication()
                .getCredentials()
                .toString();
            template.header("Authorization", "Bearer " + token);
        };
    }
}

@FeignClient(name = "user-service", configuration = FeignConfig.class)
public interface UserClient {
    // Methods
}
```

**Example - Custom Error Decoder:**
```java
public class CustomErrorDecoder implements ErrorDecoder {
    @Override
    public Exception decode(String methodKey, Response response) {
        switch (response.status()) {
            case 404:
                return new ResourceNotFoundException("Resource not found");
            case 403:
                return new UnauthorizedException("Access denied");
            default:
                return new Exception("Generic error");
        }
    }
}

@Configuration
public class FeignConfig {
    @Bean
    public ErrorDecoder errorDecoder() {
        return new CustomErrorDecoder();
    }
}
```

---

## Key Points

- **Enable Feign**: Add `@EnableFeignClients` on main class or configuration class.
- **Logging**: Configure logging level (NONE, BASIC, HEADERS, FULL) for debugging. Full logging helpful during development.
- **Retry**: Feign supports retry via Retryer interface. Configure max attempts, backoff period.
- **Compression**: Enable request/response compression for large payloads: `feign.compression.request.enabled=true`.
- **Testing**: Mock Feign clients easily in tests; interfaces simplify mocking vs. RestTemplate.
