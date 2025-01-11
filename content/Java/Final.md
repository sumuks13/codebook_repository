tags : [[Java]]

The **final keyword** in java is used to restrict the user. The java final keyword can be used in many context. Final can be:

1. variable -> stop value change
2. method -> stop overriding
3. class -> stop inheritance

If you want to create a variable that is initialized at the time of creating object and once initialized may not be changed, it is useful. For example PAN CARD number of an employee. If you try to modify, you get Output: Compile Time Error

### Q) Can we initialize blank final variable?

Yes, but only in constructor.

```java
class Bike10{  
	final int speedlimit;//blank final variable  
	
	Bike10(){
		speedlimit=70;
		System.out.println(speedlimit);  
	}
}
```

### static blank final variable

A static final variable that is not initialized at the time of declaration is known as static blank final variable. It can be initialized only in static block.

### Example of static blank final variable


```java
class A{  
	static final int data;//static blank final variable  
	static{ 
		data=50;
	}  
   public static void main(String args[]){  
     System.out.println(A.data);  
	}  
}
```
