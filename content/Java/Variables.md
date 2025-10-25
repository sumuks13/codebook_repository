tags : [[Java/Java]]

#### 1) Local Variable

A variable declared inside the body of the method is called local variable. You can use this variable only within that method.

A local variable cannot be defined with "static" keyword.

#### 2) Instance Variable

A variable declared inside the class but outside the body of the method, is called an instance variable. 
its value is instance-specific and is not shared among instances.

#### 3) Static variable

A variable that is declared as static is called a static variable. 
You can create a single copy of the static variable and share it among all the instances of the class. Memory allocation for static variables happens only once when the class is loaded in the memory.

## Typecasting

Converting one data type into another.

#### 1. Widening

```java
int a=10;  
float f=a;
```
#### 2. Narrowing

```java
float f=10.5f;  
//int a=f; //Compile time error  
int a=(int)f;
```
## Overflow

```run-java
public class Simple {
	public static void main(String[] args) {
		//Overflow
		int a=130;
		byte b=(byte)a;
		System.out.println(a);
		System.out.println(b);
	}
}
```
