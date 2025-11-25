---
{"publish":true,"title":"Spring Boot Actuators","cssclasses":""}
---

## Definitions

**Actuator**: Spring Boot feature exposing operational management endpoints via HTTP or JMX. Provides runtime visibility into application metrics, health, environment, and configuration.

**Endpoint**: Specific actuator functionality exposed as HTTP endpoint (e.g., `/actuator/health`, `/actuator/metrics`). Configurable for inclusion/exclusion and web/JMX exposure.

**Health Indicator**: Component reporting health status of application and dependent systems (database, cache, message broker). Returns UP, DOWN, or OUT_OF_SERVICE status.

**Metrics**: Application measurements (counters, gauges, timers, histograms) tracked for performance monitoring and analysis. Integrated with Micrometer for vendor-neutral metrics.

**Custom Actuator**: User-defined endpoint or health indicator extending Spring Boot's monitoring capabilities for application-specific metrics and checks.

---

## Q&A

**What is the purpose of Spring Boot Actuator?**

Actuator provides production-ready monitoring and management capabilities. Exposes application internals (health, metrics, logs, environment) via REST endpoints or JMX, enabling operational teams to monitor performance, diagnose issues, and manage application runtime behavior without code changes.

**How do you enable Actuator in a Spring Boot application?**

Add `spring-boot-starter-actuator` dependency to POM/Gradle. Actuator automatically configures upon startup, exposing endpoints based on environment (web/JMX). Configure exposure via `management.endpoints.web.exposure.include` property (whitelist endpoints to expose). By default, only `/actuator/health` and `/actuator/info` are exposed.

**What are some important Actuator endpoints?**

Common endpoints include: `/actuator/health` (application health status), `/actuator/metrics` (list available metrics), `/actuator/env` (environment properties), `/actuator/loggers` (logger configuration), `/actuator/threaddump` (thread information), `/actuator/httptrace` (HTTP request/response history), `/actuator/configprops` (configuration properties).

**How do you customize health indicators?**

Implement `HealthIndicator` interface with `health()` method returning `Health.up()` or `Health.down()` with details. Component name derives from class name (e.g., `DatabaseHealthIndicator` → `/actuator/health/database`). Spring automatically detects and includes custom indicators.

**Can you disable specific Actuator endpoints?**

Yes, configure `management.endpoints.web.exposure.exclude` property to hide specific endpoints. Example: `management.endpoints.web.exposure.exclude=env,configprops`. Alternatively, use `management.endpoint.*.enabled=false` to disable individual endpoints. Security can further restrict access.

**How do you add custom metrics to your application?**

Use `MeterRegistry` (from Micrometer) to create counters, timers, gauges. Inject `MeterRegistry` bean, then call methods like `registry.counter("myapp.counter").increment()`. Metrics automatically exported to monitoring systems (Prometheus, Grafana, etc.). Custom metrics appear in `/actuator/metrics` endpoint.

---

## Code Examples

**Example - Maven Dependency:**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**Example - Enable Actuator Endpoints (application.yml):**
```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,metrics,env,loggers
  endpoint:
    health:
      show-details: when-authorized
```

**Example - Custom Health Indicator:**
```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    @Override
    public Health health() {
        try {
            // Check database connectivity
            boolean isHealthy = checkDatabase();
            if (isHealthy) {
                return Health.up().withDetail("database", "Connected").build();
            } else {
                return Health.down().withDetail("database", "Connection failed").build();
            }
        } catch (Exception e) {
            return Health.down().withException(e).build();
        }
    }
}
```

**Example - Custom Metrics:**
```java
@RestController
public class MyController {
    private final MeterRegistry meterRegistry;
    
    public MyController(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
    }
    
    @GetMapping("/api/data")
    public ResponseEntity<?> getData() {
        meterRegistry.counter("myapp.api.calls").increment();
        meterRegistry.timer("myapp.api.duration").record(() -> {
            // Business logic
        });
        return ResponseEntity.ok("data");
    }
}
```

---

## Key Points

- **Security**: Actuator endpoints are operational; restrict access via Spring Security in production.
- **Performance**: Enabling all endpoints may expose sensitive information; whitelist only necessary endpoints.
- **Health Details**: `show-details` property controls what health information is exposed (never, when-authorized, always).
- **Metrics Integration**: Micrometer supports multiple backends; configure via `management.metrics.export.*` properties.
- **Custom Endpoints**: Implement `@Endpoint` annotation for custom actuator endpoints beyond predefined ones.
