An object is an instance of a class. A class is a template or blueprint from which objects are created. 


An object in Java is the physical as well as a logical entity, whereas, a class in Java is a logical entity only.

![[Pasted image 20240415085227.png]]

class in Java can contain:
	Fields
	Methods
	Constructors
	Blocks
	Nested class and interface

## Method in Java

In Java, a method is a function defined under a class which is used to expose the behavior of an object.

### Ways to initialize object 

By reference variable
	Student s1=new Student();  
	  s1.id=101;  
	  s1.name="Sonoo";  
  
By method
	Student s1=new Student();  
	  s1.insertRecord(111,"Karan");  
  
By constructor
	Constructor name must be the same as its class name
	A Constructor must have no explicit return type
	A Java constructor cannot be abstract, static, final, and synchronized

constructor is called "Default Constructor" when it doesn't have any parameter
constructor which has a specific number of parameters is called a parameterized constructor.

![[Pasted image 20240415090316.png]]



![[Pasted image 20240415085832.png]]

### Anonymous object

Anonymous simply means nameless. An object which has no reference is known as an anonymous object. It can be used at the time of object creation only.

If you have to use an object only once, an anonymous object is a good approach. For example:

new Calculation().fact(); //anonymous object  

