---
{"publish":true,"title":"Spring Cloud Config: Centralized Configuration","cssclasses":""}
---

## Definitions

**Spring Cloud Config**: Centralized configuration management for distributed systems. Externalizes configuration from applications into central repository (Git, filesystem).

**Config Server**: Server exposing configuration via REST API. Pulls configuration from backend (Git), serves to client applications.

**Config Client**: Application fetching configuration from Config Server on startup. Reads properties dynamically without rebuilding/redeploying.

**Git Backend**: Most common Config Server backend. Stores configuration files in Git repository. Enables versioning, auditing, rollback.

**Profile-Based Configuration**: Different configuration per environment (dev, test, prod). Naming convention: `application-{profile}.yml`.

**Encryption/Decryption**: Config Server encrypts sensitive properties (passwords, API keys). Clients receive decrypted values; encryption keys managed securely.

**Refresh Scope**: Allows applications reloading configuration at runtime without restart. Use `@RefreshScope` annotation; trigger refresh via actuator endpoint.

---

## Q&A

**Why use centralized configuration instead of local properties?**

Centralized config separates configuration from code; change config without rebuild/redeploy. Enables consistent configuration across multiple instances, environment-specific configs managed centrally, auditing/versioning via Git, dynamic updates without downtime. Especially valuable in microservices with many services.

**How does Config Server work with Git?**

Config Server clones Git repository on startup, serves files from local copy. On client request, server reads appropriate file (`application.yml`, `service-name.yml`), applies profile overlays, returns merged configuration. Git backend enables versioning; rollback by switching branches/tags.

**What is the bootstrap.yml file and why is it needed?**

`bootstrap.yml` loads before `application.yml`; contains Config Server URL and application name. Spring Cloud needs this early configuration finding Config Server, fetching application-specific properties. Without bootstrap phase, application can't locate Config Server.

**How do you handle sensitive data in Config Server?**

Encrypt sensitive values using Config Server's encrypt endpoint (`/encrypt`). Store encrypted values in Git with `{cipher}` prefix. Config Server decrypts automatically before serving to clients. Alternative: use Vault backend for secrets management. Never store plain-text secrets in Git.

**Can you refresh configuration without restarting application?**

Yes, use `@RefreshScope` on beans needing dynamic config updates. Trigger refresh via `/actuator/refresh` endpoint (POST request). Config Client fetches new properties, recreates `@RefreshScope` beans. Use Spring Cloud Bus for cluster-wide refresh with single trigger.

**What are the different Config Server backends?**

Git (most common, versioned), SVN, Filesystem (local files), Vault (secrets), JDBC (database), Redis, AWS S3. Git preferred for version control and audit trail. Vault for sensitive secrets. Filesystem for simple use cases.

---

## Code Examples

**Example - Config Server Setup:**
```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}

// application.yml
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/myorg/config-repo
          default-label: main
          search-paths: '{application}'
```

**Example - Config Client Setup:**
```java
@SpringBootApplication
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

// bootstrap.yml (loads first)
spring:
  application:
    name: user-service
  cloud:
    config:
      uri: http://localhost:8888
      fail-fast: true
  profiles:
    active: dev

// No application.yml needed; properties from Config Server
```

**Example - Git Repository Structure:**
```
config-repo/
├── application.yml          # Common to all services
├── application-dev.yml      # Dev environment
├── application-prod.yml     # Prod environment
├── user-service.yml         # User service specific
├── user-service-dev.yml     # User service dev
└── order-service.yml        # Order service specific
```

**Example - Refresh Scope:**
```java
@RestController
@RefreshScope  // Enables dynamic property refresh
public class ConfigController {
    
    @Value("${app.message:Default Message}")
    private String message;
    
    @GetMapping("/message")
    public String getMessage() {
        return message;
    }
}

// Trigger refresh:
// curl -X POST http://localhost:8080/actuator/refresh
```

**Example - Encryption:**
```bash
# Encrypt value
curl http://localhost:8888/encrypt -d mysecretpassword

# Result: AQATBzMvqL6x...
# Store in Git as:
# database.password: '{cipher}AQATBzMvqL6x...'

# Config Server decrypts automatically
```

---

## Key Points

- **Bootstrap Context**: Bootstrap loads before application context; required for Config Server discovery.
- **Fail Fast**: Set `spring.cloud.config.fail-fast=true` for production; application fails startup if Config Server unavailable.
- **Caching**: Config Server caches Git repository; configure refresh interval balancing freshness vs. performance.
- **Security**: Secure Config Server with Basic Auth or OAuth2. Don't expose publicly; internal network only.
- **Spring Cloud Bus**: Combines with Config Server for cluster-wide configuration refresh via message broker (RabbitMQ, Kafka).
