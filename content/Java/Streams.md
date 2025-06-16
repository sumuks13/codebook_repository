
### **Find the longest string in a list of strings**

```run-java
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

```run-java
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




