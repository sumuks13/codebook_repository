tags : [[Spring/Spring Boot]], [[Spring/Spring Framework]], [[Spring/IOC Container]],[[Design Patterns]], [[Spring/Autowiring]]

### Inversion Of Control (IOC) and Dependency Injection

**IOC** - inversion of control is the principle where the control flow of a program is inverted: instead of the programmer controlling the flow of a program, the external sources (framework, services, other components) take control of it. objects do not create other objects on which they rely to do their work. Instead, they get the objects that they need from an outside source (for example, an xml configuration file).

**Dependency-Injection (DI)** -  pattern is a more specific version of IoC pattern, where implementations are passed into an object through constructors/setters/service lookups, which the object will "depend" on in order to behave correctly.

These are the design patterns that are used to remove dependency from the programming code.
- makes the code loosely coupled so easy to maintain
- makes the code easy to test

Without IOC - If you need to create an Employee, you always need to create a new Address.

```java
class Employee{  
	Address address;  
	Employee(){  
		address=new Address();  
	}  
}
```

With IOC - If you create an Employee, you don't have to create a new Address.

```java
class Employee{  
	Address address;  
	Employee(Address address){  
		this.address=address;  
	}  
}
```

In Spring framework, IOC container is responsible to inject the dependency. We provide metadata to the IOC container either by XML file or annotation.


```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans  
     xmlns="http://www.springframework.org/schema/beans"  
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
     xmlns:p="http://www.springframework.org/schema/p"  
     xsi:schemaLocation="http://www.springframework.org/schema/beans  
                http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">  

 <bean id="studentbean" class="com.javatpoint.Student">  
 <property name="name" value="Vimal Jaiswal"></property>  
 </bean>  

</beans>
```

The **bean** element is used to define the bean for the given class. 

The **property** sub element of bean specifies the property of the Student class named name. The value specified in the property element will be set in the Student class object by the IOC container.

### Dependency Lookup

The Dependency Lookup is an approach where we get the resource after demand. There can be various ways to get the resource for example:

```java
//new keyword
A obj = new AImpl();
```

```java
//we get the resource by calling the static factory method getA()
A obj = A.getA();
```

```java
// by JNDI (Java Naming Directory Interface)
Context ctx = new InitialContext();  
Context environmentCtx = (Context) ctx.lookup("java:comp/env");  
A obj = (A)environmentCtx.lookup("A");
```
#### Problems of Dependency Lookup

- **tight coupling** The dependency lookup approach makes the code tightly coupled. If resource is changed, we need to perform a lot of modification in the code.
- **Not easy for testing** This approach creates a lot of problems while testing the application especially in black box testing.

### Two ways to perform Dependency Injection in Spring framework

- By Constructor
- By Setter method

Constructor :

```xml
<bean id="e" class="com.javatpoint.Employee">  
<constructor-arg value="10" type="int"></constructor-arg>  
</bean>
```

```java
Resource r=new ClassPathResource("applicationContext.xml");  
BeanFactory factory=new XmlBeanFactory(r);  

Employee s=(Employee)factory.getBean("e");
```

Setter :

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans  
    xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xmlns:p="http://www.springframework.org/schema/p"  
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
                http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">  

<bean id="obj" class="com.javatpoint.Employee">  
 <property name="id">  
 <value>20</value>  
 </property>  
 <property name="name">  
 <value>Arun</value>  
 </property>  
 <property name="city">  
 <value>ghaziabad</value>  
 </property>  

 </bean>  
</beans>
```

```java
Resource r=new ClassPathResource("applicationContext.xml");  
BeanFactory factory=new XmlBeanFactory(r);  

Employee e=(Employee)factory.getBean("obj");
```

## Constructor Injection with Dependent Object

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans  
    xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xmlns:p="http://www.springframework.org/schema/p"  
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
                http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">  

<bean id="a1" class="com.javatpoint.Address">  
 <constructor-arg value="ghaziabad"></constructor-arg>  
 <constructor-arg value="UP"></constructor-arg>  
 <constructor-arg value="India"></constructor-arg>  
 </bean>  

 <bean id="e" class="com.javatpoint.Employee">  
 <constructor-arg value="12" type="int"></constructor-arg>  
 <constructor-arg value="Sonoo"></constructor-arg>  
 <constructor-arg>  
 <ref bean="a1"/>  
 </constructor-arg>  
 </bean>  
</beans>
```

The **ref** attribute is used to define the reference of another object, such way we are passing the dependent object as an constructor argument.

## Constructor Injection with Map Example

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans  
    xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xmlns:p="http://www.springframework.org/schema/p"  
    xsi:schemaLocation="http://www.springframework.org/schema/beans   
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">  

<bean id="q" class="com.javatpoint.Question">  
 <constructor-arg value="11"></constructor-arg>  
 <constructor-arg value="What is Java?"></constructor-arg>  
 <constructor-arg>  
 <map>  
 <entry key="Java is a Programming Language"  value="Ajay Kumar"></entry>  
 <entry key="Java is a Platform" value="John Smith"></entry>  
 <entry key="Java is an Island" value="Raj Kumar"></entry>  
 </map>  
 </constructor-arg>  
</bean>  

</beans>
```

The **entry** attribute of **map** is used to define the key and value information.

## Difference between constructor and setter injection

1. **Partial dependency**: can be injected using setter injection but it is not possible by constructor. Suppose there are 3 properties in a class, having 3 arg constructor and setters methods. In such case, if you want to pass information for only one property, it is possible by setter method only.
2. **Overriding**: Setter injection overrides the constructor injection. If we use both constructor and setter injection, IOC container will use the setter injection.
3. **Changes**: We can easily change the value by setter injection. It doesn't create a new bean instance always like constructor. So setter injection is flexible than constructor injection.

## Dependency Injection with Factory Method in Spring

1. **factory-method:** represents the factory method that will be invoked to inject the bean. A method that returns instance of a class is called **factory method**.
2. **factory-bean:** represents the reference of the bean by which factory method will be invoked. It is used if factory method is non-static.

### Factory Method Types

1. **static factory method** that returns instance of **its own** class (singleton design pattern)

```xml
<bean id="a" class="com.javatpoint.A" factory-method="getA"></bean>
```

2. **static factory method** that returns instance of **another** class

```xml
<bean id="b" class="com.javatpoint.A" factory-method="getB"></bean>
```

3. **non-static factory** method that returns instance of **another** class.

```xml
<bean id="a" class="com.javatpoint.A"></bean>  
<bean id="b" class="com.javatpoint.A" factory-method="getB" factory-bean="a"></bean>
```


```java
public class A {  
	private static final A obj = new A();
	  
	private A(){
		System.out.println("private constructor");
	}  
	public static A getA(){  
	    System.out.println("factory method ");  
	    return obj;
	}
}

//main
ApplicationContext context= new ClassPathXmlApplicationContext("applicationContext.xml");  

A a=(A)context.getBean("a");
```

