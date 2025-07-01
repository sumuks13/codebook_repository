tags: [[Spring Framework]]

A Spring MVC is a Java framework which is used to build web applications. 
It follows the Model-View-Controller design pattern. 

It implements all the basic features of a core spring framework like Inversion of Control, Dependency Injection.

**DispatcherServlet** is the class that receives the incoming request and maps it to the right resource such as controllers, models, and views.

![[Pasted image 20240608122124.png]]

- **Model** - A model contains the data of the application. A data can be a single object or a collection of objects.

- **Controller** - A controller contains the business logic of an application. Here, the `@Controller` annotation is used to mark the class as the controller.

- **View** - A view represents the provided information in a particular format. Generally, JSP+JSTL is used to create a view page. Although spring also supports other view technologies such as Apache Velocity, Thymeleaf and FreeMarker.

- **Front Controller** - In Spring Web MVC, the DispatcherServlet class works as the front controller. It is responsible to manage the flow of the Spring MVC application.

# Understanding the flow of Spring Web MVC

![Spring MVC Tutorial](https://static.javatpoint.com/sppages/images/flow-of-spring-web-mvc.png)

- As displayed in the figure, all the incoming request is intercepted by the DispatcherServlet that works as the front controller.
- The DispatcherServlet gets an entry of handler mapping from the XML file and forwards the request to the controller.
- The controller returns an object of ModelAndView.
- The DispatcherServlet checks the entry of view resolver in the XML file and invokes the specified view component.

# Advantages of Spring MVC Framework:-

- **Separate roles** - The Spring MVC separates each role, where the model object, controller, command object, view resolver, DispatcherServlet, validator, etc. can be fulfilled by a specialized object.
- **Light-weight** - It uses light-weight servlet container to develop and deploy your application.
- **Powerful Configuration** - It provides a robust configuration for both framework and application classes that includes easy referencing across contexts, such as from web controllers to business objects and validators.
- **Rapid development** - The Spring MVC facilitates fast and parallel development.
- **Reusable business code** - Instead of creating new objects, it allows us to use the existing business objects.
- **Easy to test** - In Spring, generally we create JavaBeans classes that enable you to inject test data using the setter methods.
- **Flexible Mapping** - It provides the specific annotations that easily redirect the page.

# Spring MVC Model Interface

In Spring MVC, the model works as a container that contains the objects/data of the application.

It is required to place the **Model** interface in the controller part of the application. The object of **HttpServletRequest** reads the information provided by the user and pass it to the **Model** interface. Now, a view page easily accesses the data from the model part.

```java
@Controller  
public class HelloController {  
  
    @RequestMapping("/hello")  
    public String display(HttpServletRequest req,Model m)  
    {  
        //read the provided form data  
        String name=req.getParameter("name");  
        String pass=req.getParameter("pass");  
        if(pass.equals("admin"))  
        {  
            String msg="Hello "+ name;  
            //add a message to the model  
            m.addAttribute("message", msg);  
            return "viewpage";  
        }  
        else  
        {  
            String msg="Sorry "+ name+". You entered an incorrect password";  
            m.addAttribute("message", msg);  
            return "errorpage";  
        }     
    }  
}  
```

- The **HttpServletRequest** is used to read the HTML form data provided by the user.
- The **Model** contains the request data and provides it to view page.

# RequestParam


**@RequestParam** annotation is used to read the form data and bind it automatically to the parameter present in the provided method. So, it ignores the requirement of **HttpServletRequest** object to read the provided data.

```java
@RequestMapping("/hello")  
    //read the provided form data  
public String display(@RequestParam("name") String name,
					  @RequestParam("pass") String pass,Model m)  
{}
```
https://www.javatpoint.com/spring-mvc-form-tag-library