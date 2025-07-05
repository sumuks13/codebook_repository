
```java
List<Integer> listOfIntegers = Arrays.asList(1,2,10,3,4,5,6,7,8,1,4,9,10,4,8,11,12,13); 
```

```java
List<String> listOfStrings = Arrays.asList("apple", "banana", "cherry", "pear", "pineapple", "mango", "grapes"); 
```

### **Given a list of integers, print each number using Stream API**

```java
listOfIntegers.forEach(i -> System.out.println(i));

listOfIntegers.stream()
			.forEach(i -> System.out.println(i));  
```

### **From a list of integers, get all even numbers**

```java
listOfIntegers.stream()
			.filter(i -> i % 2 == 0)
			.collect(Collectors.toList());

listOfIntegers.stream()
			.filter(i -> i % 2 == 0)
			.toList();
```

### **Given a list of String, convert all to uppercase**

```java
listOfStrings.stream()
			.map(s -> s.toUpperCase())
			.toList();
```

### **From a list of strings, find those that start with "p"**

```java
listOfStrings.stream()
			.filter(s -> s.startsWith("p"))
			.toList();
```

### **Count how many numbers in a list are greater than 10**

```java
listOfIntegers.stream()
			.filter(i -> i > 10)
			.collect(Collectors.counting());
			
listOfIntegers.stream()
			.filter(i -> i > 10)
			.count();
```

### **Sort a list of integers in descending order using Stream**

```java
listOfIntegers.stream()
			.sorted(Comparator.reverseOrder())
			.toList(); 
```

### **Find the max number in a list using Stream**

```java
listOfIntegers.stream()
			.max(Comparator.naturalOrder());

listOfIntegers.stream()
			.min(Comparator.reverseOrder()));
```

### **Get sum of a list of numbers**

```java
listOfIntegers.stream()
			.reduce((m,n) -> m+n));  

listOfIntegers.stream()
			.mapToInt(n -> n)
			.sum();
```

### **From a list of integers, remove duplicates using Stream**

```java
listOfIntegers.stream()
			.distinct()
			.toList();
			
listOfIntegers.stream()
			.collect(Collectors.toSet());
```

### **Given a list of strings, return a list of lengths of each string**

```java
listOfStrings.stream()
			.map(s -> s.length())
			.collect(Collectors.toList());
```

### **Find the longest string in a list of strings**

```java
import java.util.*;

public class Test{ 
	public static void main(String[] args){
		List<String> strings = Arrays.asList("apple", "banana", "cherry", "mango", "grapefruit");
		Optional<String> result = strings.stream()
										 .max(Comparator.comparingInt(s -> s.length()));
		
		System.out.println(result.get());
	}
}
```

### **Calculate the average age of a list of Person objects**

```java
import java.util.*;

public class Test { 
	public static void main(String[] args){
		List<Person> persons = Arrays.asList(  
		    new Person("Alice", 25),  
		    new Person("Bob", 30),  
		    new Person("Charlie", 35)  
		);  
		
		double averageAge = persons.stream()  
		                          .mapToInt(Person::getAge)  
		                          .average()  
		                          .orElse(0);
		
		System.out.println(averageAge);
	}
}

class Person{
	private String name;
	private int age;

	public Person(String name, int age){
		super();
		this.name = name;
		this.age = age;
	}

	public int getAge(){
		return this.age;
	}
}

```

> [!Quote] References
> - https://medium.com/@mehar.chand.cloud/java-stream-coding-interview-questions-part-1-dc39e3575727




