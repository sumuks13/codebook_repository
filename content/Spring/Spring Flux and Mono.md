tags : [[Spring/Spring Boot]], [[Spring/Spring WebClient]]

In simple terms, Mono and Flux are implementations of the Publisher interface in Spring WebFlux. They are used for reactive programming in web applications. Here's a breakdown of what they are and how they are used:

1. Mono: 
   - Mono is similar to the Optional class in Java, as it can contain 0 or 1 value.
   - It is used when we expect a maximum of one result from a computation, database query, or external service call.
   - An example of creating a Mono:
     ```java
     Mono<String> mono = Mono.just("Hello");
     Mono<String> emptyMono = Mono.empty();
     ```

2. Flux: 
   - Flux is similar to a List in Java, as it can contain any number of values.
   - It is used when we expect multiple results from a computation, database query, or external service call.
   - An example of creating a Flux:
     ```java
     Flux<String> flux = Flux.just("A", "B", "C");
     Flux<String> arrayFlux = Flux.fromArray(new String[]{"A", "B", "C"});
     Flux<String> iterableFlux = Flux.fromIterable(Arrays.asList("A", "B", "C"));
     ```

3. Spring WebClient:
   - WebClient is a reactive web client introduced in Spring 5.
   - It is used for performing web requests in a non-blocking and reactive manner.
   - WebClient replaces the classic RestTemplate in Spring WebFlux applications.
   - WebClient supports both synchronous and asynchronous operations, making it suitable for applications running on a Servlet Stack.
   - An example of using WebClient to send an HTTP POST request:
     ```java
     WebClient webClient = WebClient.create("http://localhost:3000");
     Employee createdEmployee = webClient.post()
         .uri("/employees")
         .header(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
         .body(Mono.just(employee), Employee.class)
         .retrieve()
         .bodyToMono(Employee.class)
         .block();
     ```

These concepts are part of the Spring WebFlux framework, which provides reactive programming support for web applications. Spring WebFlux internally uses Project Reactor and its publisher implementations, Mono and Flux.

Sources:
- [Source 2](https://www.baeldung.com/java-reactor-flux-vs-mono)
- [Source 3](https://howtodoinjava.com/spring-webflux/spring-webflux-tutorial/)
- [Source 4](https://www.baeldung.com/spring-5-webclient)
- [Source 5](https://howtodoinjava.com/spring-webflux/webclient-get-post-example/)

_Flux_ is useful when we need to handle zero to many or potentially infinite results. We can think of a Twitter feed as an example.
When we know that the results are returned all at once – we can use _Mono