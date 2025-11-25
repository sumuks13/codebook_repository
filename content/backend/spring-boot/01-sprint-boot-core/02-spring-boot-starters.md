---
{"publish":true,"title":"Spring Boot Starters","cssclasses":""}
---

## Definitions

**Spring Boot Starter**: Pre-configured dependency descriptor following `spring-boot-starter-*` naming convention. Aggregates related libraries and their compatible versions for specific functionality.

**Transitive Dependencies**: Dependencies automatically included when you add a starter. The starter manages version compatibility, so you don't manually specify each library.

**Dependency Exclusion**: Mechanism to remove unwanted dependencies brought in by a starter. Useful when starter includes libraries you don't need or want different versions.

**POM (Project Object Model)**: Maven configuration file (pom.xml) defining project structure, dependencies, build settings, and plugins.

**Custom Starter**: User-created starter following Spring Boot conventions to package and distribute common configurations and dependencies for organizational use.

---

## Q&A

**Why use Spring Boot starters instead of individual dependencies?**

Starters eliminate "dependency hell" by providing curated, compatible library sets for common use cases. They reduce manual version management, ensure best practices out of the box, and simplify dependency resolution. Example: `spring-boot-starter-web` includes Spring Web, Spring Boot autoconfiguration, Tomcat, Jackson, and logging—all compatible versions.

**What are some commonly used Spring Boot starters?**

Common starters include: `spring-boot-starter-web` (web applications), `spring-boot-starter-data-jpa` (database access), `spring-boot-starter-security` (authentication/authorization), `spring-boot-starter-test` (testing), `spring-boot-starter-actuator` (monitoring), `spring-boot-starter-cloud-config` (cloud configuration).

**How do you exclude unwanted dependencies from a starter?**

Use your build tool's exclusion mechanism. For Maven, add `<exclusions>` block within `<dependency>`. For Gradle, use `exclude group:` syntax. Example: exclude Tomcat from web starter to use Jetty instead. This allows fine-grained control over your classpath without breaking starter benefits.

**Is it possible to create custom Spring Boot starters?**

Yes, organizations create custom starters for shared configurations and libraries. A custom starter typically contains auto-configuration classes and metadata files in `META-INF/spring.factories` or `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` to enable auto-configuration discovery.

**What happens if a starter brings conflicting dependency versions?**

Build tools resolve conflicts using their default rules (typically nearest dependency or explicit version declaration wins). Generally, starters are well-tested and conflicts are rare. If conflicts occur, explicitly declare preferred version in your POM/Gradle file to override starter's transitive dependency.

**Can you use Spring Boot without starters?**

Yes, you can manually add dependencies, but you lose autoconfiguration benefits and must manage versions, compatibility, and configuration yourself. Not recommended; starters provide significant convenience and best practices that make development faster and more reliable.

---

## Code Examples

**Example - Maven POM with Starters:**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

**Example - Gradle Starters:**
```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

**Example - Exclude and Replace Embedded Server:**
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

**Example - Gradle Exclude:**
```gradle
dependencies {
    implementation('org.springframework.boot:spring-boot-starter-web') {
        exclude group: 'org.springframework.boot', module: 'spring-boot-starter-tomcat'
    }
    implementation 'org.springframework.boot:spring-boot-starter-jetty'
}
```

---

## Key Points

- **Starter Naming**: All starters follow `spring-boot-starter-*` pattern for consistency and discoverability.
- **Version Management**: Starters manage transitive dependency versions; override only when necessary and documented.
- **Exclusion Practice**: Exclude dependencies sparingly; understand why before excluding to avoid breaking functionality.
- **BOM (Bill of Materials)**: Use `spring-boot-dependencies` BOM in parent POM for version consistency across projects.
- **Start.spring.io**: Use Spring Initializr web interface to generate projects with pre-selected starters rather than manual configuration.
