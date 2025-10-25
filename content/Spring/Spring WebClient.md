tags : [[Spring/Spring Boot]], [[Spring/Spring RestTemplate]]
URL : [howtodoinjava.com](https://howtodoinjava.com/spring-webflux/webclient-get-post-example/#2-creating-a-spring-webclient-instance)

**Spring `WebClient`** is a **non-blocking and reactive web client** to perform HTTP requests. **The `WebClient` has been added in  Spring 5  and provides the fluent functional-style _API_ for sending HTTP requests and handling the responses.**

Before Spring 5, [_RestTemplate_](https://howtodoinjava.com/spring-boot2/resttemplate/spring-restful-client-resttemplate-example/) has been the primary technique for client-side HTTP accesses, which is part of the _Spring MVC_ project. **In [Spring 5](https://howtodoinjava.com/series/spring-tutorials/) and [Spring 6](https://howtodoinjava.com/java/whats-new-spring-6-spring-boot-3/), using `_WebClient_` is the recommended approach.**

To use WebClient api, we must have _[spring-boot-starter-webflux](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-webflux)_ module imported into the project.

---
pom.xml
```xml
<dependency>     
	<groupId>org.springframework.boot</groupId>     
	<artifactId>spring-boot-starter-webflux</artifactId> 
</dependency>
```

---
## 1. Creating a Spring _WebClient_ Instance

To create `_WebClient_` bean, we can follow any one of the given approaches.

### 1.1. Using _WebClient.create()_

The `create()` method is an overloaded method and can optionally accept a base URL for requests.

```java
WebClient webClient1 = WebClient.create();  //with empty URI
WebClient webClient2 = WebClient.create("https://client-domain.com");
```

### 1.2. Using _WebClient.Builder_ API

We can also build the client using the _DefaultWebClientBuilder_ class, which uses builder pattern style fluent-API.

```java
WebClient webClient2 = WebClient.builder()
        .baseUrl("http://localhost:3000")
        .defaultCookie("cookie-name", "cookie-value")
        .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
        .build();
```