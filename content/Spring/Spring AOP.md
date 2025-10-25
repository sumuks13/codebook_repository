tags : [[[Spring Framework]]

[Spring AOP Tutorial](https://www.javatpoint.com/spring-aop-tutorial)

Aspect-oriented programming (AOP) is **an approach to programming that allows global properties of a program to determine how it is compiled into an executable program**. AOP can be used with object-oriented programming ( OOP ).

AOP breaks the program logic into distinct parts (called **concerns**). It is used to increase modularity by **cross-cutting concerns**.

A **cross-cutting concern** is a concern that can affect the whole application and should be centralized in one location in code as possible, such as 
- transaction management, 
- authentication, 
- logging, 
- user input validations,
- security etc.

## AOP Concepts and Terminology

- Join point
- Advice
- Pointcut
- Introduction
- Target Object
- Aspect
- Interceptor
- AOP Proxy
- Weaving 

### Join point

Join point is any point in your program such as method execution, exception handling, field access etc. Spring supports only method execution join point.

### Advice

Advice represents an action taken by an aspect at a particular join point. There are different types of advices:

- **Before Advice**: it executes before a join point.
- **After Returning Advice**: it executes after a joint point completes normally.
- **After Throwing Advice**: it executes if method exits by throwing an exception.
- **After (finally) Advice**: it executes after a join point regardless of join point exit whether normally or exceptional return.
- **Around Advice**: It executes before and after a join point.

![[attachments/Pasted image 20240602134033.png]]

- **MethodBeforeAdvice** interface extends the **BeforeAdvice** interface.
- **AfterReturningAdvice** interface extends the **AfterAdvice** interface.
- **ThrowsAdvice** interface extends the **AfterAdvice** interface.
- **MethodInterceptor** interface extends the **Interceptor** interface. It is used in around advice.
### Pointcut

It is an expression language of AOP that matches join points.

```java
@Pointcut("execution(* Operation.*(..))")  
private void doSomething() {}  

The name of the pointcut expression is doSomething(). 
It will be applied on all the methods of Operation class regardless of return type.
```

```java
@Pointcut("execution(public * *(..))")  

It will be applied on all the public methods.
```

```java
@Pointcut("execution(public Employee.set*(..))")  

It will be applied on all the public setter methods of Employee class.
```

```java
@Pointcut("execution(int Operation.*(..))")  

It will be applied on all the methods of Operation class that returns int value.
```

### Introduction

It means introduction of additional method and fields for a type. It allows you to introduce new interface to any advised object.

### Target Object

It is the object i.e. being advised by one or more aspects. It is also known as proxied object in spring because Spring AOP is implemented using runtime proxies.

### Aspect

It is a class that contains advices, joinpoints etc.

### Interceptor

It is an aspect that contains only one advice.

### AOP Proxy

It is used to implement aspect contracts, created by AOP framework. It will be a JDK dynamic proxy or CGLIB proxy in spring framework.

### Weaving

It is the process of linking aspect with other application types or objects to create an advised object. Weaving can be done at compile time, load time or runtime. Spring AOP performs weaving at runtime.

### AOP Implementations

AOP implementations are provided by:

1. AspectJ
2. Spring AOP
3. JBoss AOP

Spring AOP:

```java
package com.javatpoint;  
 public class A {  
	 public void m(){
		 System.out.println("actual business logic");
	}  
 }
```

```java
import java.lang.reflect.Method;  
import org.springframework.aop.MethodBeforeAdvice;  
public class BeforeAdvisor implements MethodBeforeAdvice{  
    @Override  
    public void before(Method method, Object[] args, Object target)throws Throwable {  
        System.out.println("additional concern before actual logic");  
    }  
}  
```

```java
public class Test {  
	public static void main(String[] args) {  
	    Resource r=new ClassPathResource("applicationContext.xml");  
	    BeanFactory factory=new XmlBeanFactory(r);  
      
	    A a=factory.getBean("proxy",A.class);  
	    a.m();  
	}  
}
//output
additional concern before actual logic  
actual business logic
```

AspectJ

There are two ways to use Spring AOP AspectJ implementation:

1. By annotation.
2. By xml configuration.

1. **@Aspect** declares the class as aspect.
2. **@Pointcut** declares the pointcut expression.

1. **@Before** declares the before advice. It is applied before calling the actual method.
2. **@After** declares the after advice. It is applied after calling the actual method and before returning result.
3. **@AfterReturning** declares the after returning advice. It is applied after calling the actual method and before returning result. But you can get the result value in the advice.
4. **@Around** declares the around advice. It is applied before and after calling the actual method.
5. **@AfterThrowing** declares the throws advice. It is applied if actual method throws exception.

```java
public  class Operation{  
    public void msg(){System.out.println("msg method invoked");}  
    public int m(){System.out.println("m method invoked");return 2;}  
    public int k(){System.out.println("k method invoked");return 3;}  
}  
```

```java
@Aspect  
public class TrackOperation{  
    @Pointcut("execution(* Operation.*(..))")  
    public void k(){}//pointcut name  
      
    @Before("k()")//applying pointcut on before advice  
    public void myadvice(JoinPoint jp)//it is advice (before advice)  
    {  
        System.out.println("additional concern");    
    }  
}  
```

```java
public class Test{  
    public static void main(String[] args){  
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");  
        Operation e = (Operation) context.getBean("opBean");  
        System.out.println("calling msg...");  
        e.msg();  
        System.out.println("calling m...");  
        e.m();  
        System.out.println("calling k...");  
        e.k();  
    }  
}  

//Output
1. calling msg...  
2. additional concern  
3. msg() method invoked  
4. calling m...  
5. additional concern  
6. m() method invoked  
7. calling k...  
8. additional concern  
9. k() method invoked
```

2. By XML :

1. **aop:before** It is applied before calling the actual business logic method.
2. **aop:after** It is applied after calling the actual business logic method.
3. **aop:after-returning** it is applied after calling the actual business logic method. It can be used to intercept the return value in advice.
4. **aop:around** It is applied before and after calling the actual business logic method.
5. **aop:after-throwing** It is applied if actual business logic method throws exception.

```xml
<aop:aspectj-autoproxy />  
  
<bean id="opBean" class="com.javatpoint.Operation">   </bean>  
<bean id="trackAspect" class="com.javatpoint.TrackOperation"></bean>  
          
<aop:config>  
  <aop:aspect id="myaspect" ref="trackAspect" >  
     <!-- @Before -->  
     <aop:pointcut id="pointCutBefore"   expression="execution(* com.javatpoint.Operation.*(..))" />  
     <aop:before method="myadvice" pointcut-ref="pointCutBefore" />  
  </aop:aspect>  
</aop:config>
```

