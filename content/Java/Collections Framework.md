tags: [[Java]], [[Collection]]

 **Collections framework** provides an architecture to store and manipulate group of objects.

Java Collection framework provides many interfaces (Set, List, Queue, Deque) and classes ([ArrayList](https://www.javatpoint.com/java-arraylist), Vector, [LinkedList](https://www.javatpoint.com/java-linkedlist), [PriorityQueue](https://www.javatpoint.com/java-priorityqueue), HashSet, LinkedHashSet, TreeSet).

#### What is a framework in Java

- It provides readymade architecture.
- It represents a set of classes and interfaces.
- It is optional.

![[Pasted image 20240521054600.png]]

## Iterator interface

Iterator interface provides the facility of iterating the elements in a forward direction only

#### Methods of Iterator interface

There are only three methods in the Iterator interface. They are:

|No.|Method|Description|
|---|---|---|
|1|public boolean hasNext()|It returns true if the iterator has more elements otherwise it returns false.|
|2|public Object next()|It returns the element and moves the cursor pointer to the next element.|
|3|public void remove()|It removes the last elements returned by the iterator. It is less used.|

## Iterable Interface

The Iterable interface is the root interface for all the collection classes. The Collection interface extends the Iterable interface and therefore all the subclasses of Collection interface also implement the Iterable interface.

It contains only one abstract method. i.e.,

```
Iterator<T> iterator()  
```

It returns the iterator over the elements of type T.

### Iterating ArrayList using Iterator

```run-java
import java.util.*;  
public class ArrayListExample2{  
 public static void main(String args[]){  
  ArrayList<String> list=new ArrayList<String>();//Creating arraylist  
  list.add("Mango");//Adding object in arraylist    
  list.add("Apple");    
  list.add("Banana");    
  list.add("Grapes");    
  //Traversing list through Iterator  
  Iterator itr=list.iterator();//getting the Iterator  
  while(itr.hasNext()){//check if iterator has the elements  
   System.out.println(itr.next());//printing the element and move to next  
  }  
 }  
}  
```
