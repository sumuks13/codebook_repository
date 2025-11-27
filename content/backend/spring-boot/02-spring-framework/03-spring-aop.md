---
{"publish":true,"title":"AOP (Aspect-Oriented Programming)","cssclasses":""}
---

## Definitions

**Aspect-Oriented Programming (AOP)**: Programming paradigm separating cross-cutting concerns (logging, security, transactions) from business logic. Promotes modularization of concerns affecting multiple classes.

**Aspect**: Modular unit containing cross-cutting concern logic. Defined using `@Aspect` annotation; combines pointcuts and advice.

**Pointcut**: Expression identifying join points where advice should execute. Written in AspectJ syntax; matches method executions, field accesses, etc.

**Advice**: Code executed when pointcut matches. Types: @Before (pre-execution), @After (post-execution), @AfterReturning (after successful return), @AfterThrowing (after exception), @Around (wrap execution).

**Join Point**: Specific point in program execution where advice can be applied. In Spring AOP limited to method execution.

**Weaving**: Process of applying aspects to target objects. Spring uses dynamic proxies (runtime weaving); not compile-time or load-time.

---

## Q&A

**Why use AOP instead of duplicating cross-cutting concern code?**

AOP eliminates code duplication for logging, security, transaction management by centralizing concern logic into reusable aspects. Applied declaratively without modifying business code. Improves maintainability, readability, and enables applying concerns across methods consistently. Changes to concern logic update aspect once, affecting all join points automatically.

**What are the different types of advice in Spring AOP?**

@Before executes before method; @After executes after method regardless of outcome; @AfterReturning executes after successful return; @AfterThrowing executes after exception thrown; @Around wraps method execution, can control execution, return value, and exceptions. @Around most powerful but should be used carefully; @Before/@After typically sufficient for most concerns.

**How does Spring implement AOP?**

Spring uses dynamic proxies created at runtime. For classes implementing interfaces, uses JDK dynamic proxies; for classes without interfaces, uses CGLIB proxies creating subclasses. Proxies intercept method calls, applying advice before delegating to target object. This runtime approach (weaving) simpler than compile-time/load-time alternatives but slightly less performant.

**Can you apply AOP to private methods?**

No, Spring AOP only applies to public methods due to proxy-based implementation. Private methods are not overrideable; proxies cannot intercept them. For private method concerns, use other techniques (direct method calls, separate services). This is a key limitation; AspectJ compile-time weaving supports private method aspects.

**What's the difference between @Before and @Around advice?**

@Before executes before method and cannot prevent execution or modify arguments. @Around wraps entire method, can prevent execution via `joinPoint.proceed()` control, modify return value, catch exceptions. @Around more powerful but verbose; use @Before when just executing before method without needing control.

**How do you access method parameters and return value in advice?**

Use `JoinPoint` parameter to access method info: `joinPoint.getArgs()` returns parameters, `joinPoint.getSignature()` returns method signature. In @AfterReturning, use `returning` parameter to capture return value. In @Around, use `ProceedingJoinPoint.proceed()` to execute method and capture return value.

---

## Code Examples

**Example - Simple Logging Aspect:**
```java
@Aspect
@Component
public class LoggingAspect {
    
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        System.out.println("Entering method: " + methodName);
    }
    
    @After("execution(* com.example.service.*.*(..))")
    public void logAfter(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        System.out.println("Exiting method: " + methodName);
    }
}
```

**Example - @Around for Performance Monitoring:**
```java
@Aspect
@Component
public class PerformanceMonitoringAspect {
    
    @Around("execution(* com.example.service.*.*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) 
            throws Throwable {
        long startTime = System.currentTimeMillis();
        try {
            return joinPoint.proceed();
        } finally {
            long endTime = System.currentTimeMillis();
            String methodName = joinPoint.getSignature().getName();
            System.out.println(methodName + " took " + 
                (endTime - startTime) + "ms");
        }
    }
}
```

**Example - Security Aspect:**
```java
@Aspect
@Component
public class SecurityAspect {
    
    @Before("@annotation(com.example.annotation.RequiresAuth)")
    public void checkAuthentication(JoinPoint joinPoint) {
        if (!isUserAuthenticated()) {
            throw new SecurityException("User not authenticated");
        }
    }
    
    private boolean isUserAuthenticated() {
        return SecurityContextHolder.getContext()
            .getAuthentication() != null;
    }
}
```

**Example - @AfterReturning to Capture Return Value:**
```java
@Aspect
@Component
public class CachingAspect {
    private Map<String, Object> cache = new HashMap<>();
    
    @AfterReturning(pointcut = "execution(* find*(..))", 
                    returning = "result")
    public void cacheResult(JoinPoint joinPoint, Object result) {
        String key = joinPoint.getSignature().getName();
        cache.put(key, result);
        System.out.println("Cached: " + key);
    }
}
```

**Example - Pointcut with Custom Annotation:**
```java
@Aspect
@Component
public class TransactionAspect {
    
    @Around("@annotation(com.example.annotation.Transactional)")
    public Object handleTransaction(ProceedingJoinPoint joinPoint) 
            throws Throwable {
        System.out.println("Starting transaction");
        try {
            Object result = joinPoint.proceed();
            System.out.println("Committing transaction");
            return result;
        } catch (Exception e) {
            System.out.println("Rolling back transaction");
            throw e;
        }
    }
}
```

---

## Key Points

- **Proxy-Based Limitations**: Spring AOP (runtime proxies) only supports public method interception; AspectJ (compile-time) supports private methods, field access, constructor execution.
- **Performance**: Each aspect application adds overhead; use sparingly for performance-critical code. Measure impact in production scenarios.
- **Debugging**: Proxy-based aspects complicate debugging; stack traces include proxy frames. Use debugging tools aware of proxies.
- **Pointcut Syntax**: AspectJ pointcut syntax powerful but complex; test thoroughly. Common mistakes: forgetting parentheses, incorrect package patterns.
- **Ordering**: Multiple aspects on same join point execute in order determined by `@Order` annotation; control execution sequence for predictable behavior.
