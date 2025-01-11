tags : [[Java]], [[Collections Framework]]

A Collection means a group of objects. 

![[Pasted image 20240521054600.png]]
## Collection Interface

The Collection interface is the interface which is implemented by all the classes in the collection framework. It declares the methods that every collection will have.

Some of the methods of Collection interface are Boolean add ( Object obj), Boolean addAll ( Collection c), void clear(), etc. which are implemented by all the subclasses of Collection interface.

### List Interface

List interface is the child interface of Collection interface. It inhibits a list type data structure in which we can store the **ordered** collection of objects. It can have **duplicate** values.

List interface is implemented by the classes ArrayList, LinkedList, Vector, and Stack.

### ArrayList

The ArrayList class implements the List interface. It uses a dynamic array to store the duplicate element of different data types. The ArrayList class maintains the insertion order and is non-synchronized.

ArraliList.size() : The size represents the total number of elements present in the array. If the list has no elements added then the size of the array list is zero.

Capacity represents the total number of elements the array list can contain. Therefore, the capacity of an array list is always greater than or equal to the size of the array list. When we add an element to the array list, it checks whether the size of the array list has become equal to the capacity or not. If yes, then the capacity of the array list increases.

```java
//Converting ArrayList to Array  
String[] array = fruitList.toArray(new String[fruitList.size()]);
String[] array = fruitList.toArray();
```

## <mark style="background: #ADCCFFA6;">LinkedList</mark>

LinkedList implements the Collection interface. It uses a doubly linked list internally to store the elements. It can store the duplicate elements. It maintains the insertion order and is not synchronized. In LinkedList, the manipulation is fast because no shifting is required.

```java
//Traversing the list of elements in reverse order  
Iterator i=ll.descendingIterator();
```

## <mark style="background: #ADCCFFA6;">Vector</mark>

Vector uses a dynamic array to store the data elements. It is similar to ArrayList. However, It is synchronized and contains many methods that are not the part of Collection framework.

## <mark style="background: #ADCCFFA6;">Stack</mark>

The stack is the subclass of Vector. It implements the last-in-first-out data structure, i.e., Stack. The stack contains all of the methods of Vector class and also provides its methods like boolean push(), boolean peek(), boolean push(object o), which defines its properties.


## <mark style="background: #FFB86CA6;">Queue Interface</mark>

Queue interface maintains the first-in-first-out order. There are various classes like PriorityQueue, Deque, and ArrayDeque which implements the Queue interface.

## <mark style="background: #FFB86CA6;">PriorityQueue</mark>

The PriorityQueue class implements the Queue interface. It holds the elements or objects which are to be processed by their priorities. PriorityQueue doesn't allow null values to be stored in the queue.

## <mark style="background: #FFB86CA6;">Deque Interface</mark>

Deque interface extends the Queue interface. In Deque, we can remove and add the elements from both the side. Deque stands for a double-ended queue which enables us to perform the operations at both the ends.

## <mark style="background: #FFB86CA6;">ArrayDeque</mark>

ArrayDeque class implements the Deque interface. It facilitates us to use the Deque. Unlike queue, we can add or delete the elements from both the ends.

ArrayDeque is faster than ArrayList and Stack and has no capacity restrictions.

## <mark style="background: #FFB86CA6;">Set Interface</mark>

Set Interface in Java is present in java.util package. It extends the Collection interface. It represents the unordered set of elements which doesn't allow us to store the duplicate items. We can store at most one null value in Set. 
Set is implemented by HashSet, LinkedHashSet, and TreeSet.

## <mark style="background: #FFB86CA6;">HashSet</mark>

HashSet class implements Set Interface. It represents the collection that uses a hash table for storage. Hashing is used to store the elements in the HashSet. It contains unique items.

## <mark style="background: #FFB86CA6;">LinkedHashSet</mark>

LinkedHashSet class represents the LinkedList implementation of Set Interface. It extends the HashSet class and implements Set interface. Like HashSet, It also contains unique elements. It maintains the insertion order and permits null elements.

## <mark style="background: #FFF3A3A6;">SortedSet Interface</mark>

SortedSet is the alternate of Set interface that provides a total ordering on its elements. The elements of the SortedSet are arranged in the increasing (ascending) order.

## <mark style="background: #FFF3A3A6;">TreeSet</mark>

Java TreeSet class implements the Set interface that uses a tree for storage. Like HashSet, TreeSet also contains unique elements. However, the access and retrieval time of TreeSet is quite fast. The elements in TreeSet stored in ascending order.
### ClassCast Exception in TreeSet

If we add an object of the class that is not implementing the Comparable interface, the ClassCast Exception is raised.

It is because the TreeSet maintains the sorting order, and for doing the sorting the comparison of different objects that are being inserted in the TreeSet is must, which is accomplished by implementing the Comparable interface. So all the Objects used for TreeSet must implement Comparable interface.

## Java ListIterator Interface

ListIterator Interface is used to traverse the element in a backward and forward direction.

### hods of Java ListIterator Interface:

|Method|Description|
|---|---|
|void add(E e)|This method inserts the specified element into the list.|
|boolean hasNext()|This method returns true if the list iterator has more elements while traversing the list in the forward direction.|
|E next()|This method returns the next element in the list and advances the cursor position.|
|int nextIndex()|This method returns the index of the element that would be returned by a subsequent call to next()|
|boolean hasPrevious()|This method returns true if this list iterator has more elements while traversing the list in the reverse direction.|
|E previous()|This method returns the previous element in the list and moves the cursor position backward.|
|E previousIndex()|This method returns the index of the element that would be returned by a subsequent call to previous().|
|void remove()|This method removes the last element from the list that was returned by next() or previous() methods|
|void set(E e)|This method replaces the last element returned by next() or previous() methods with the specified element.|

## When to use what

![[Pasted image 20240525052708.png]]

![[Pasted image 20240525053052.png]]

## Lists

| Lists Comparison Table                                   | Add/remove element in the beginning | Add/remove element in the middle | Add/remove element in the end | Get i-th element (random access) | Find element                | Traversal order |
| -------------------------------------------------------- | ----------------------------------- | -------------------------------- | ----------------------------- | -------------------------------- | --------------------------- | --------------- |
| [_ArrayList_](https://www.baeldung.com/java-arraylist)   | _O(n)_                              | _O(n)_                           | _O(1)_                        | _O(1)_                           | _O(n), O(log(n)) if sorted_ | as inserted     |
| _[LinkedList](https://www.baeldung.com/java-linkedlist)_ | _O(1)_                              | _O(1)_                           | _O(1)_                        | _O(n)_                           | _O(n)_                      | as inserted     |
- When the rate of addition or removal rate is more than the read scenarios, then go for the LinkedList. On the other hand, when the frequency of the read scenarios is more than the addition or removal rate, then ArrayList takes precedence over LinkedList.

- Since the elements of an ArrayList are stored more compact as compared to a LinkedList; therefore, the ArrayList is more cache-friendly as compared to the LinkedList. Thus, chances for the cache miss are less in an ArrayList as compared to a LinkedList. Generally, it is considered that a LinkedList is poor in cache-locality.

- Memory overhead in the LinkedList is more as compared to the ArrayList. It is because, in a LinkedList, we have two extra links (next and previous) as it is required to store the address of the previous and the next nodes, and these links consume extra space. Such links are not present in an ArrayList.

## Sets

| Sets Comparison Table                                          | Add element      | Remove element   | Find element | Traversal order                                      |
| -------------------------------------------------------------- | ---------------- | ---------------- | ------------ | ---------------------------------------------------- |
| _[HashSet](https://www.baeldung.com/java-hashset)_             | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | random, scattered by the hash function               |
| _[LinkedHashSet](https://www.baeldung.com/java-linkedhashset)_ | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | as inserted                                          |
| _[TreeSet](https://www.baeldung.com/java-tree-set)_            | _O(log(n))_      | _O(log(n))_      | _O(log(n))_  | sorted, according to elements comparison criterion   |
| _[EnumSet](https://www.baeldung.com/java-enumset)_             | _O(1)_           | _O(1)_           | _O(1)_       | according to the definition order of the enum values |

## Queues

1. _LinkedList_, _ArrayDeque_ –  can act as the stack, queue, and dequeue data structures. Generally, _ArrayDeque_ is faster than _LinkedList_. Hence it’s the default choice
2. _[PriorityQueue](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/PriorityQueue.html) – Queue_ interface implementation backed by the binary heap data structure. Used for fast (_O(1)_) element retrieval, which has the highest priority. Addition and removal work in _O(log(n))_ time

## Maps

We use put() method to insert the Key and Value pair in the HashMap, the default size of HashMap is 16 (0 to 15).

| Maps Comparison Table                                           | Add element      | Remove element   | Find element | Traversal order                                      |
| --------------------------------------------------------------- | ---------------- | ---------------- | ------------ | ---------------------------------------------------- |
| _[HashMap](https://www.baeldung.com/java-hashmap)_              | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | random, scattered by the hash function               |
| _[LinkedHashMap](https://www.baeldung.com/java-linked-hashmap)_ | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | as inserted                                          |
| _[TreeMap](https://www.baeldung.com/java-treemap)_              | _O(log(n))_      | _O(log(n))_      | _O(log(n))_  | sorted, according to elements comparison criterion   |
| _[EnumMap](https://www.baeldung.com/java-enum-map)_             | _O(1)_           | _O(1)_           | _O(1)_       | according to the definition order of the enum values |

