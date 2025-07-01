tags: [[Spring Boot]]
references  : https://www.baeldung.com/spring-mvc-and-the-modelattribute-annotation

### Method Level

when used at Method level inside @Controller Class, it binds the data to all @RequestMapping methods.

```java
@ModelAttribute("genres")
public List<String> populateGenres() {
   return Arrays.asList("Science Fiction", "Drama", "Mystery", "Horror");
}

@ModelAttribute(Model model)
public List<String> populateData() {
   model.addAttribute("defaultText", "Hello World!);
}

```

### Method Argument Level

It can be used for arguments, which will automatically bind a form data into an object;

```java
@RequestMapping(value = "/addEmployee", method = RequestMethod.POST) 
public String submit(@ModelAttribute("employee") Employee employee){ 
	//Code that uses the employee object 
	return "employeeView"; 
}
```


This Annotation can be used in collaboration with @ControllerAdvice. This will add the model attributes to all endpoints defined in all @Controllers.
