---
{"publish":true,"title":"Logs","cssclasses":""}
---

## Definitions

**Logging**: Systematic recording of application events, errors, and state changes for debugging, monitoring, and auditing purposes.

**Log Levels**: Hierarchical severity indicators: TRACE (most verbose), DEBUG (development), INFO (significant events), WARN (potential issues), ERROR (failures), FATAL (system crash).

**Logger Hierarchy**: Tree structure where child loggers inherit parent configuration. Root logger at top; package-specific loggers inherit levels, appenders.

**Correlation ID**: Unique identifier linking all logs for single request across services. Propagated via HTTP headers, message metadata.

**Async Logging**: Non-blocking log writing to file/network. Improves performance; logs buffered, written by separate thread.

---

## Q&A

**Why use structured logging instead of plain text logs?**

Structured logs (JSON) enable programmatic querying, filtering, aggregation. Search `user_id=123 AND level=ERROR` vs. regex on plain text. Include correlation IDs linking request logs across services. Machine-readable by ELK Stack, Splunk, Grafana Loki. Essential for microservices debugging.

**What's the difference between Logback and Log4j2?**

Logback faster, native SLF4J implementation, smaller memory footprint. Log4j2 better async logging, garbage-free, plugin architecture. Logback default in Spring Boot; Log4j2 popular for high-throughput. Both support structured logging; choose based on performance requirements.

**How do you implement correlation IDs in Spring Boot microservices?**

Generate at API gateway/entry point: `UUID.randomUUID()`. Propagate via `X-Correlation-ID` HTTP header, message headers. Use MDC (Mapped Diagnostic Context) storing ID, automatically included in logs. Spring Boot interceptor/filter extracts, sets in MDC for entire request.

**When should you use different log levels?**

TRACE/DEBUG for development, troubleshooting. INFO for significant business events (user login, order created). WARN for recoverable issues (invalid input). ERROR for failures preventing normal operation. FATAL for system crashes (rare). Configure levels per environment (DEBUG dev, INFO prod).

**How do you configure rolling file appenders?**

Configure `RollingFileAppender` with `TimeBasedRollingPolicy` (daily logs), `SizeAndTimeBasedFNATP` (size + time). Set `maxFileSize`, `maxHistory`. Example: 10MB files, keep 30 days. Prevents disk exhaustion from continuous logging.

**What's async logging and when to use it?**

Async logging decouples log writing from application threads. Logs buffered, separate thread writes to file/network. Dramatically improves throughput (10x+ faster). Use for high-volume logging, production environments. Trade-off: potential log loss on crash before flush.

---

## Code Examples

**Example - Structured Logging with SLF4J & Logback:**

```java
@RestController
public class OrderController {
    private static final Logger log = LoggerFactory.getLogger(OrderController.class);
    
    @PostMapping("/orders")
    public ResponseEntity<Order> createOrder(
            @RequestHeader(value = "X-Correlation-ID", required = false) String correlationId,
            @RequestBody OrderRequest request) {
        
        MDC.put("correlationId", correlationId != null ? correlationId : UUID.randomUUID().toString());
        MDC.put("userId", request.getUserId().toString());
        
        try {
            log.info("Creating order for user {} with items {}", 
                request.getUserId(), request.getItems().size());
            
            Order order = orderService.createOrder(request);
            log.info("Order {} created successfully", order.getId());
            
            return ResponseEntity.ok(order);
        } catch (Exception e) {
            log.error("Failed to create order for user {}: {}", 
                request.getUserId(), e.getMessage(), e);
            throw e;
        } finally {
            MDC.clear();
        }
    }
}

```

**Example - Logback Configuration (logback-spring.xml):**

```xml
<configuration>
    <!-- Console Appender -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
            <providers>
                <timestamp/>
                <pattern>
                    <pattern>
                        {
                            "timestamp": "%date{yyyy-MM-dd'T'HH:mm:ss.SSSXXX}",
                            "service": "${spring.application.name:-unknown}",
                            "level": "%level",
                            "correlationId": "%X{correlationId}",
                            "logger": "%logger{36}",
                            "message": "%msg",
                            "thread": "%thread"
                        }
                    </pattern>
                </pattern>
            </providers>
        </encoder>
    </appender>
    
    <!-- Async Rolling File -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/app.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>logs/app.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern>
            <maxFileSize>10MB</maxFileSize>
            <maxHistory>30</maxHistory>
            <totalSizeCap>1GB</totalSizeCap>
        </rollingPolicy>
        <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder"/>
    </appender>
    
    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

**Example - Correlation ID Filter:**

```java
@Component
public class CorrelationIdFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        
        String correlationId = httpRequest.getHeader("X-Correlation-ID");
        if (correlationId == null) {
            correlationId = UUID.randomUUID().toString();
        }
        
        // Set in response for downstream services
        httpResponse.setHeader("X-Correlation-ID", correlationId);
        
        // Put in MDC for logging
        MDC.put("correlationId", correlationId);
        
        try {
            chain.doFilter(request, response);
        } finally {
            MDC.clear();
        }
    }
}

```

---

## Key Points

- **Correlation IDs**: Always propagate across services; single pane debugging for requests.
- **Log Levels**: DEBUG/INFO dev, INFO/WARN prod. Avoid DEBUG in production (performance).
- **Structured Format**: JSON with timestamp, service, level, correlation ID, message.
- **Async Logging**: Use for high-throughput; configure queue size, overflow policy.
- **Retention**: Size + time-based rolling (10MB + 30 days). Prevent disk exhaustion.
- **Centralization**: Forward to ELK, Splunk, Loki. Query across all services.
