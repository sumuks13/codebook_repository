tags : [[Spring/Spring Boot]]

The `@Controller` annotation is a specialization of the `@Component` annotation.
The handler methods in these classes return a view name, which is resolved to a view technology file (like JSP or Thymeleaf) by a view resolver.

`@RestController` is a specialized version of the `@Controller` annotation. It includes the `@Controller` and `@ResponseBody` annotations


 @Controller is used for serving views (HTML pages)
  @RestController is used where responses are data (usually in JSON or XML format).