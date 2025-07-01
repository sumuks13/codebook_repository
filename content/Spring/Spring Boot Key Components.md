tags : [[Spring Boot]]

Spring Boot Framework has mainly four major Components.
- Spring Boot Starters
- Spring Boot AutoConfigurator
- Spring Boot CLI
- Spring Boot Actuator

Other components:
- Spring Initilizr
- Spring Boot IDEs

![[Pasted image 20240602212648.png]]

**Spring Boot Starter:**

- The main responsibility of Spring Boot Starter is to combine a group of common or related dependencies into single dependencies.
- Spring Boot Starter reduces defining many dependencies.
- Spring Boot Starter simplifies project build dependencies.

**Spring Boot AutoConfigurator**:

- Most of the Spring IO Platform (Spring Framework) Critics opinion is that "To develop a Spring-based application requires lot of configuration (Either XML Configuration or Annotation Configuration).
- The solution to this problem is Spring Boot AutoConfigurator. 
- The main responsibility of Spring Boot AutoConfigurator is to reduce the Spring Configuration.

 - For instance, if we want to declare a Spring MVC application using Spring IO Platform, then we need to define lot of XML Configuration like views, view resolvers etc. But if we use Spring Boot Framework, then we dont need to define those XML Configuration. Spring Boot AutoConfigurator will take of this. 

- If we use @SpringBootApplication annotation at class level, then Spring Boot AutoConfigurator will automatically add all required annotations to Java Class ByteCode.

```java
SpringBootApplication = Configuration + ComponentScan + EnableAutoConfiguration
```

**Spring Boot CLI**

- Spring Boot CLI(Command Line Interface) is a Spring Boot software to run and test Spring Boot applications from command prompt. 
- When we run Spring Boot applications using CLI, then it internally uses Spring Boot Starter and Spring Boot AutoConfiguration components to resolve all dependencies and execute the application. 
- We can run even Spring Web Applications with simple Spring Boot CLI Commands. Spring Boot CLI has introduced a new “spring” command to execute Groovy Scripts from command prompt.

**Spring Boot Actuator**

Spring Boot Actuator components gives many features, but two major features are
- Providing Management Endpoints to Spring Boot Applications. ( like hostname - localhost and port - 8080)
- Spring Boot Applications Metrics.


