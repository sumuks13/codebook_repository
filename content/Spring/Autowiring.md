tags: [[Spring]] [[Dependency Injection]]

Autowiring feature of spring framework enables you to inject the object dependency **implicitly**. It internally uses setter or constructor injection.

Advantages:
- It requires the **less code** because we don't need to write the code to inject the dependency explicitly.
Disadvantages:
- No control of programmer.
- It can't be used for primitive and string values.

## Autowiring Modes

| No. | Mode        | Description                                                                                                                                                             |
| --- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1)  | no          | It is the default autowiring mode. It means no autowiring bydefault.                                                                                                    |
| 2)  | byName      | The byName mode injects the object dependency according to name of the bean. In such case, property name and bean name must be same. It internally calls setter method. |
| 3)  | byType      | The byType mode injects the object dependency according to type. So property name and bean name can be different. It internally calls setter method.                    |
| 4)  | constructor | The constructor mode injects the dependency by calling the constructor of the class. It calls the constructor having large number of parameters.                        |
| 5)  | autodetect  | It is deprecated since Spring 3.                                                                                                                                        |

## 1) byName autowiring mode

In case of byName autowiring mode, bean id and reference name must be same.
It internally uses setter injection.

```xml
<bean id="b" class="org.sssit.B"></bean>  
<bean id="a" class="org.sssit.A" autowire="byName"></bean>  
```

But, if you change the name of bean, it will not inject the dependency.
Let's see the code where we are changing the name of the bean from b to b1.

```xml
<bean id="b1" class="org.sssit.B"></bean>  
<bean id="a" class="org.sssit.A" autowire="byName"></bean>
```

## 2) byType autowiring mode

In case of byType autowiring mode, bean id and reference name may be different. But there must be only one bean of a type.

It internally uses setter injection.

```xml
<bean id="b1" class="org.sssit.B"></bean>  
<bean id="a" class="org.sssit.A" autowire="byType"></bean>  
```

In this case, it works fine because you have created an instance of B type. It doesn't matter that you have different bean name than reference name.

But, if you have multiple bean of one type, it will not work and throw exception.

```xml
<bean id="b1" class="org.sssit.B"></bean>  
<bean id="b2" class="org.sssit.B"></bean>  
<bean id="a" class="org.sssit.A" autowire="byName"></bean>  
```

## 3) constructor autowiring mode

In case of constructor autowiring mode, spring container injects the dependency by highest parameterized constructor.

If you have 3 constructors in a class, zero-arg, one-arg and two-arg then injection will be performed by calling the two-arg constructor.

```xml
<bean id="b" class="org.sssit.B"></bean>  
<bean id="a" class="org.sssit.A" autowire="constructor"></bean>
```

## 4) no autowiring mode

In case of no autowiring mode, spring container doesn't inject the dependency by autowiring.

```xml
<bean id="b" class="org.sssit.B"></bean>  
<bean id="a" class="org.sssit.A" autowire="no"></bean>
```

