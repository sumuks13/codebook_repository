---
{"publish":true,"title":"Spring Boot: Embedded Server","cssclasses":""}
---

## Definitions

**Embedded Server**: Web server (Tomcat, Jetty, Undertow) packaged within Spring Boot application JAR. Eliminates external server deployment; application runs as standalone executable JAR.

**Standalone Application**: Self-contained executable JAR containing application code, dependencies, and embedded server. Deploys and runs without external application server infrastructure.

**Servlet Container**: Java component managing servlet lifecycle, HTTP request handling, and session management. Tomcat, Jetty, Undertow are servlet containers.

**Server Configuration**: Settings controlling embedded server behavior (port, context path, SSL, thread pools, session timeout). Configured via `application.properties` or `application.yml`.

**Fat JAR**: Executable JAR containing application code, all dependencies, and embedded server. Produced by Spring Boot's Maven/Gradle plugin.

---

## Q&A

**Why does Spring Boot include an embedded server?**

Embedded servers eliminate complex deployment infrastructure, enabling Spring Boot applications to run with single `java -jar` command. Simplifies DevOps, enables containerization (Docker), supports microservices architecture where each service is independently deployable and runnable.

**How do you change the embedded server port?**

Set `server.port` property in `application.properties` or `application.yml`. Default is 8080. Example: `server.port=9090`. Can also pass as command-line argument: `java -jar app.jar --server.port=9090`. Zero value (`server.port=0`) assigns random available port, useful for testing.

**Can you switch from Tomcat to Jetty or Undertow?**

Yes, exclude `spring-boot-starter-tomcat` from `spring-boot-starter-web` and add preferred server starter. For Jetty: add `spring-boot-starter-jetty`. For Undertow: add `spring-boot-starter-undertow`. Only one embedded server can be active; inclusion of multiple causes Spring Boot to choose based on classpath priority.

**How do you configure SSL/HTTPS for embedded server?**

Generate keystore, then configure in `application.properties`: `server.ssl.key-store=classpath:keystore.p12`, `server.ssl.key-store-password=password`, `server.ssl.key-store-type=PKCS12`. Application runs on HTTPS by default. Useful for testing; production typically uses reverse proxy (Nginx, AWS ELB) for SSL termination.

**What are the performance differences between embedded servers?**

Tomcat (default) is mature, widely used, good general-purpose performance. Jetty is lightweight, lower memory footprint, good for resource-constrained environments. Undertow (Red Hat) is high-performance, lowest latency, uses non-blocking I/O. Choice depends on performance requirements, team familiarity, and resource constraints.

**Can you disable embedded server and use external server?**

Yes, set `spring.main.web-application-type=none` to disable embedded server, or set it to `servlet` for embedding but `reactive` for reactive stack. For external deployment, package as WAR instead of JAR, add `spring-boot-starter-tomcat` with `provided` scope, and deploy to standalone server.

---

## Code Examples

**Example - Configure Embedded Server (application.yml):**
```yaml
server:
  port: 9090
  servlet:
    context-path: /api
  tomcat:
    threads:
      max: 200
      min-spare: 10
    max-connections: 10000
```

**Example - Switch to Jetty (Maven pom.xml):**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

**Example - Enable SSL (application.properties):**
```properties
server.ssl.key-store=classpath:keystore.p12
server.ssl.key-store-password=mypassword
server.ssl.key-store-type=PKCS12
server.port=8443
```

**Example - Customize Server Port (Command Line):**
```bash
java -jar myapp.jar --server.port=8888
```

---

## Key Points

- **Simplicity**: Single JAR simplifies deployment; no external server setup required.
- **Immutability**: Fat JAR contains exact dependencies; consistent across environments.
- **Container-Friendly**: Embedded server enables easy containerization with Docker.
- **Thread Pool Tuning**: Configure `tomcat.threads.max` based on expected concurrency and server resources.
- **Production Deployment**: For high-traffic, use reverse proxy (Nginx) in front of embedded server for SSL, load balancing, caching.
