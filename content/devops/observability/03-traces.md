---
{"publish":true,"title":"Traces (Distributed Tracing)","cssclasses":""}
---

## Definitions

**Distributed Tracing**: Observability technique tracking requests across microservices. Shows complete request journey, latency breakdown, error locations.

**Span**: Single operation within trace. Named unit of work (HTTP request, DB query). Contains start/end time, tags, logs.

**Trace**: Collection of spans forming complete request journey. Root span at entry point, child spans for downstream calls.

**Trace ID**: Unique identifier for entire request. Links all spans, logs, metrics for single request. 128-bit random value.

**Span ID**: Unique identifier per span within trace. Parent-child relationships form trace tree.

**OpenTelemetry**: CNCF standard unifying traces, metrics, logs. Collector receives, processes, exports to backend (Jaeger, Zipkin).

**Sampler**: Controls which traces collected. Head-based (first span), tail-based (high latency). Balance completeness vs. volume.

---

## Q&A

**Why is distributed tracing essential for microservices?**

Microservices generate distributed logs across dozens of services. Correlation IDs help but lack timing, causality. Traces show request path, latency per service, bottlenecks, failures. Waterfall view reveals slow DB calls, chatty services, cascading failures.

**How does trace context propagation work?**

Entry service generates trace ID, span ID. HTTP headers (`traceparent`, `tracestate`) carry context to downstream services. Each service creates child span, propagates context. OpenTelemetry standardizes propagation (W3C Trace Context). gRPC, message queues supported.

**What's the difference between Jaeger and Zipkin?**

Both open-source tracing backends. Jaeger better storage (Cassandra, Elasticsearch), sampling strategies, UI. Zipkin simpler, JSON storage, easier setup. Jaeger CNCF project, more active development. Choose Jaeger for production, Zipkin for simple setups.

**How do you implement tracing in Spring Boot?**

Add `spring-cloud-sleuth` or `micrometer-tracing-bridge-brave`. Auto-instruments HTTP clients (RestTemplate, WebClient, Feign), Spring MVC, databases. Configure `spring.sleuth.otlp.enabled=true` for OpenTelemetry. Export to Jaeger (`spring.zipkin.base-url`).

**What sampling strategies exist for distributed tracing?**

Always-on (100%, high volume), probabilistic (1% of requests), rate-limiting (first 100/min), tail-based (slow requests). Production: 1-5% + 100% errors + tail sampling (p99). Balance completeness, storage costs, performance overhead.

**How do you correlate traces with logs and metrics?**

Use trace ID in structured logs (`traceId: abc123`). Metrics tagged with trace ID (rare). OpenTelemetry Context Propagator extracts trace context for logs. ELK queries: `traceId:abc123 AND service:order-service`. Complete request story.

---

## Code Examples

**Example - Manual Span Creation:**

```java
@Service
public class OrderService {
    private final Tracer tracer;
    
    public OrderService(Tracer tracer) {
        this.tracer = tracer;
    }
    
    public Order processOrder(OrderRequest request) {
        Span span = tracer.nextSpan().name("order.process");
        try (Tracer.SpanInScope ws = tracer.withSpanInScope(span.start())) {
            span.tag("user.id", request.getUserId().toString());
            
            // Business logic
            User user = userClient.getUser(request.getUserId());
            span.tag("user.found", Boolean.toString(user != null));
            
            return createOrder(user, request);
        } catch (Exception e) {
            span.recordException(e);
            span.setStatus(Status.INTERNAL);
            throw e;
        } finally {
            span.end();
        }
    }
}
```

---

## Key Points

- **Always Sample Errors**: 100% error traces + probabilistic sampling.    
- **Span Naming**: Descriptive, consistent (`http.server.request`, `db.query.user`).
- **Context Propagation**: HTTP headers, gRPC metadata, message headers.
- **Storage**: Jaeger (Cassandra/ES), Zipkin (ES), Tempo (object storage).
- **Overhead**: 1-5% CPU/memory. Tune sampling for production.
- **Alerting**: Slow traces (p95 > 2s), missing services, high error rates.
