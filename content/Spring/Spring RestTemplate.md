tags : [[Spring Boot]], [[Spring WebClient]]

Spring RestTemplate is a Spring Framework class for making HTTP requests to external services or APIs.

1. **HTTP Methods:** supports various HTTP methods such as GET, POST, PUT, DELETE, etc.
    
2. **Request and Response Conversion:** automatically convert HTTP requests and responses to and from various data formats like JSON, XML, etc., using message converters.
    
3. **Error Handling:** It handles HTTP error responses and allows you to define custom error handlers.
    
4. **URI Template Expansion:** RestTemplate supports URI template expansion, making it easy to construct dynamic URLs for API endpoints.
    
5. **Basic Authentication:** You can easily set up basic authentication for your requests when accessing secured APIs.
    
6. **Request Interceptors:** RestTemplate allows you to add request interceptors to modify or inspect outgoing requests before they are sent.
    
7. **Response Extraction:** It provides methods to extract data from the HTTP response, making it convenient to process the received data.

```JAVA
RestTemplate restTemplate = new RestTemplate();
String apiUrl = "https://api.example.com/data";
ResponseEntity<String> response = restTemplate.getForEntity(apiUrl, String.class);

if (response.getStatusCode().is2xxSuccessful()) {
    String responseBody = response.getBody();
    System.out.println(responseBody);
}
```


> [!Question] RestTemplate Blocking Client vs WebClient Non-Blocking Client
> 
> Under the hood, **_RestTemplate_ uses the Java Servlet API, which is based on the thread-per-request model.**
> 
> This means that the thread will block until the web client receives the response. The problem with the blocking code is due to each thread consuming some amount of memory and CPU cycles, 
> 
> Consequently, the application will create many threads, which will exhaust the thread pool or occupy all the available memory.
> 
> [[Spring WebClient]] uses an asynchronous, non-blocking solution provided by the Spring Reactive framework.






