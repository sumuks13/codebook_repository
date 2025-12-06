---
{"publish":true,"title":"Spring Cloud Eureka: Service Discovery","cssclasses":""}
---

## Definitions

**Service Discovery**: Pattern enabling microservices to locate each other dynamically. Services register themselves; clients query registry for service locations.

**Eureka Server**: Netflix service registry maintaining list of service instances. Services register on startup, send heartbeats periodically.

**Eureka Client**: Service registering with Eureka Server. Fetches registry, locates other services, load balances requests.

**Service Registration**: Process where service announces itself to Eureka on startup. Includes host, port, health URL, metadata.

**Heartbeat**: Periodic signal from service to Eureka proving it's alive. Default 30 seconds; missed heartbeats trigger deregistration.

**Service Instance**: Running copy of microservice. Multiple instances for scalability/availability; Eureka tracks all instances.

**Load Balancing**: Distributing requests across service instances. Ribbon (client-side) or Spring Cloud LoadBalancer integrates with Eureka for automatic balancing.

---

## Q&A

**Why is service discovery important in microservices?**

Microservices have dynamic IPs/ports (cloud, containers, autoscaling). Hardcoding addresses impossible; service discovery provides dynamic lookup. Services register automatically; clients query registry for current instances. Enables scaling, failover, deployment without configuration changes.

**How does Eureka service registration work?**

Service starts, reads configuration (`eureka.client.serviceUrl`), registers with Eureka Server sending metadata (name, host, port, health URL). Sends heartbeats every 30 seconds. Eureka removes services missing heartbeats (default 90 seconds). Clients fetch registry, cache locally, query for services by name.

**What's the difference between client-side and server-side discovery?**

Client-side: Client queries registry (Eureka), gets service instances, selects one (load balancing), calls directly. Server-side: Client calls load balancer (API Gateway), which queries registry, forwards request. Client-side more efficient (fewer hops); server-side simpler client logic. Spring Cloud uses client-side with Eureka.

**How do you handle Eureka Server failure?**

Configure multiple Eureka Servers (peer-to-peer replication). Clients configured with all server URLs; failover automatic. Clients cache registry; if Eureka down, use cached data (stale but functional). Eureka designed for availability over consistency (AP in CAP theorem).

**What is the purpose of Eureka's self-preservation mode?**

When network partition occurs, many services miss heartbeats simultaneously. Self-preservation mode prevents mass deregistration; Eureka stops evicting services. Prevents cascading failures from transient network issues. Disabled in development (set `eureka.server.enable-self-preservation=false`); enabled in production.

**How do you integrate Eureka with Spring Boot?**

Add `spring-cloud-starter-netflix-eureka-server` for server or `spring-cloud-starter-netflix-eureka-client` for clients. Annotate main class with `@EnableEurekaServer` (server) or `@EnableEurekaClient` (client). Configure `application.yml` with Eureka URLs, service name. Services auto-register on startup.

---

## Code Examples

**Example - Eureka Server Configuration:**
```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}

// application.yml
server:
  port: 8761

eureka:
  client:
    register-with-eureka: false  # Server doesn't register itself
    fetch-registry: false
  server:
    enable-self-preservation: true
```

**Example - Eureka Client (Microservice):**
```java
@SpringBootApplication
@EnableEurekaClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

// application.yml
spring:
  application:
    name: user-service

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
    register-with-eureka: true
    fetch-registry: true
  instance:
    prefer-ip-address: true
    lease-renewal-interval-in-seconds: 30
    lease-expiration-duration-in-seconds: 90
```

**Example - Service-to-Service Communication:**
```java
@Configuration
public class RestTemplateConfig {
    @Bean
    @LoadBalanced  // Enables Ribbon load balancing with Eureka
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

@Service
public class OrderService {
    @Autowired
    private RestTemplate restTemplate;
    
    public User getUserDetails(Long userId) {
        // Service name instead of host:port
        String url = "http://user-service/api/users/" + userId;
        return restTemplate.getForObject(url, User.class);
    }
}
```

**Example - Multiple Eureka Servers (HA):**
```yaml
# Eureka Server 1 (peer1)
spring:
  application:
    name: eureka-server
server:
  port: 8761

eureka:
  client:
    service-url:
      defaultZone: http://peer2:8762/eureka/

---
# Eureka Server 2 (peer2)
spring:
  application:
    name: eureka-server
server:
  port: 8762

eureka:
  client:
    service-url:
      defaultZone: http://peer1:8761/eureka/
```

---

## Key Points

- **Service Names**: Use consistent naming convention (lowercase, hyphens). Service name is key for discovery.
- **Health Checks**: Configure actuator health endpoint; Eureka uses it verifying service health.
- **Zones**: Eureka supports availability zones; prefer same-zone instances for lower latency.
- **Security**: Secure Eureka dashboard and API with Spring Security in production. Don't expose publicly.
- **Caching**: Clients cache registry locally (default 30 seconds refresh). Balance freshness vs. load on Eureka.
