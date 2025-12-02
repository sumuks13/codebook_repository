---
{"publish":true,"title":"Servlet API","cssclasses":""}
---

tags: [03-web-and-architecture](backend/spring-boot/03-web-and-architecture)

## Definitions

**Servlet**: Java class handling HTTP requests and generating responses. Extends `HttpServlet`, implements request processing lifecycle.

**HttpServletRequest**: Object containing HTTP request data (headers, parameters, body, session). Provides methods accessing request information.

**HttpServletResponse**: Object for sending HTTP response (status, headers, body). Provides output stream/writer for response generation.

**Filter**: Component intercepting requests/responses before reaching servlet. Used for logging, authentication, security, compression.

**FilterChain**: Chain of filters executed sequentially. Each filter decides whether to pass control to next filter or halt processing.

**Listener**: Component responding to servlet lifecycle events (initialization, request arrival, session creation). Implements ServletContextListener, HttpSessionListener, etc.

---

## Q&A

**What is the servlet lifecycle?**

Servlet lifecycle: (1) init() called once when servlet loaded, (2) doGet()/doPost()/etc. called for each request, (3) destroy() called when servlet unloaded. init() performs one-time initialization (database connections, resource loading). doXxx() methods handle actual requests. destroy() cleanup. Spring's DispatcherServlet follows this lifecycle.

**How do filters differ from servlets?**

Servlets handle requests and generate responses directly. Filters intercept requests before reaching servlet and responses before returning to client. Multiple filters chain sequentially; each can modify request/response or halt processing. Filters used for cross-cutting concerns (logging, security, compression) without modifying servlet code.

**What can you do with HttpServletRequest?**

Access request information: `getMethod()` returns HTTP method, `getParameterNames()` returns parameter names, `getHeader(name)` returns header value, `getSession()` returns session, `getCookies()` returns cookies, `getInputStream()` reads request body. Use for extracting form data, headers, session information needed for processing.

**How do you set response headers and status codes?**

Use HttpServletResponse methods: `setStatus(int code)` sets HTTP status (200, 404, 500), `setHeader(name, value)` sets header, `addCookie(Cookie)` adds cookie. For content type: `setContentType("application/json")`. Headers must be set before writing response body. Setting headers after body started raises exception.

**What is servlet context and how is it used?**

ServletContext represents web application. Provides application-wide functionality: store shared objects, get deployment parameters, log messages. Access via `getServletContext()` in servlet/filter. Use for accessing resources, logging, storing application-level state shared across servlets.

**How do request dispatchers work?**

RequestDispatcher allows forwarding requests internally: `request.getRequestDispatcher("/page.jsp").forward(request, response)` transfers control without URL change. Alternative: `response.sendRedirect("/page.jsp")` sends browser redirect (URL changes). Forward retains request/response objects; redirect creates new request. Use forward for internal processing, redirect for external navigation.

---

## Code Examples

**Example - Simple Servlet:**
```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    @Override
    public void init() throws ServletException {
        System.out.println("HelloServlet initialized");
    }
    
    @Override
    protected void doGet(HttpServletRequest request, 
                        HttpServletResponse response) 
            throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<h1>Hello, World!</h1>");
        out.close();
    }
    
    @Override
    public void destroy() {
        System.out.println("HelloServlet destroyed");
    }
}
```

**Example - Request Parameters:**
```java
@WebServlet("/process")
public class ProcessServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, 
                         HttpServletResponse response) 
            throws ServletException, IOException {
        String name = request.getParameter("name");
        String email = request.getParameter("email");
        
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("Name: " + name);
        out.println("Email: " + email);
    }
}
```

**Example - Filter Implementation:**
```java
@WebFilter("/*")
public class LoggingFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, 
                        ServletResponse response, 
                        FilterChain chain) 
            throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        System.out.println("Request: " + httpRequest.getMethod() + 
                         " " + httpRequest.getRequestURI());
        
        chain.doFilter(request, response);  // Continue chain
        
        System.out.println("Response completed");
    }
}
```

**Example - Session Listener:**
```java
@WebListener
public class SessionListener implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent event) {
        System.out.println("Session created: " + 
                         event.getSession().getId());
    }
    
    @Override
    public void sessionDestroyed(HttpSessionEvent event) {
        System.out.println("Session destroyed: " + 
                         event.getSession().getId());
    }
}
```

---

## Key Points

- **Servlet Container**: Manages servlet lifecycle, handles HTTP requests, manages threads. Tomcat, Jetty are containers.
- **Thread Safety**: Servlets are singletons; same instance handles multiple concurrent requests. Ensure thread-safety in servlet code.
- **Request/Response Scope**: HttpServletRequest/Response available only during request processing; don't store references for later use.
- **Spring Handles Servlets**: Spring Boot abstracts away servlet details via DispatcherServlet; most developers never directly implement servlets.
- **Filter Ordering**: Filter execution order determined by filter registration order; declare in web.xml or use `@Order` annotation for ordering.
