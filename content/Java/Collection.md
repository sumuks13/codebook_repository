tags : [[Java/Java]], [[Java/Collections Framework]], [[Java/List]], [[Java/Set]], [[Java/Queue]], [[Java/Map]]

A Collection means a group of objects. 

![[Pasted image 20240521054600.png \| center ]]

### **Lists**

| Lists Comparison Table                                   | Add/remove element in the beginning | Add/remove element in the middle | Add/remove element in the end | Get i-th element (random access) | Find element                | Traversal order |
| -------------------------------------------------------- | ----------------------------------- | -------------------------------- | ----------------------------- | -------------------------------- | --------------------------- | --------------- |
| [_ArrayList_](https://www.baeldung.com/java-arraylist)   | _O(n)_                              | _O(n)_                           | _O(1)_                        | _O(1)_                           | _O(n), O(log(n)) if sorted_ | as inserted     |
| _[LinkedList](https://www.baeldung.com/java-linkedlist)_ | _O(1)_                              | _O(1)_                           | _O(1)_                        | _O(n)_                           | _O(n)_                      | as inserted     |
- When the rate of addition or removal rate is more than the read scenarios, then go for the LinkedList. On the other hand, when the frequency of the read scenarios is more than the addition or removal rate, then ArrayList takes precedence over LinkedList.

- Since the elements of an ArrayList are stored more compact as compared to a LinkedList; therefore, the ArrayList is more cache-friendly as compared to the LinkedList. Thus, chances for the cache miss are less in an ArrayList as compared to a LinkedList. Generally, it is considered that a LinkedList is poor in cache-locality.

- Memory overhead in the LinkedList is more as compared to the ArrayList. It is because, in a LinkedList, we have two extra links (next and previous) as it is required to store the address of the previous and the next nodes, and these links consume extra space. Such links are not present in an ArrayList.

### **Sets**

| Sets Comparison Table                                          | Add element      | Remove element   | Find element | Traversal order                                      |
| -------------------------------------------------------------- | ---------------- | ---------------- | ------------ | ---------------------------------------------------- |
| _[HashSet](https://www.baeldung.com/java-hashset)_             | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | random, scattered by the hash function               |
| _[LinkedHashSet](https://www.baeldung.com/java-linkedhashset)_ | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | as inserted                                          |
| _[TreeSet](https://www.baeldung.com/java-tree-set)_            | _O(log(n))_      | _O(log(n))_      | _O(log(n))_  | sorted, according to elements comparison criterion   |
| _[EnumSet](https://www.baeldung.com/java-enumset)_             | _O(1)_           | _O(1)_           | _O(1)_       | according to the definition order of the enum values |
### **Queues**

1. _LinkedList_, _ArrayDeque_ –  can act as the stack, queue, and dequeue data structures. Generally, _ArrayDeque_ is faster than _LinkedList_. Hence it’s the default choice
2. _[PriorityQueue](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/PriorityQueue.html) – Queue_ interface implementation backed by the binary heap data structure. Used for fast (_O(1)_) element retrieval, which has the highest priority. Addition and removal work in _O(log(n))_ time

### **Maps**

We use put() method to insert the Key and Value pair in the HashMap, the default size of HashMap is 16 (0 to 15).

| Maps Comparison Table                                           | Add element      | Remove element   | Find element | Traversal order                                      |
| --------------------------------------------------------------- | ---------------- | ---------------- | ------------ | ---------------------------------------------------- |
| _[HashMap](https://www.baeldung.com/java-hashmap)_              | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | random, scattered by the hash function               |
| _[LinkedHashMap](https://www.baeldung.com/java-linked-hashmap)_ | amortized _O(1)_ | amortized _O(1)_ | _O(1)_       | as inserted                                          |
| _[TreeMap](https://www.baeldung.com/java-treemap)_              | _O(log(n))_      | _O(log(n))_      | _O(log(n))_  | sorted, according to elements comparison criterion   |
| _[EnumMap](https://www.baeldung.com/java-enum-map)_             | _O(1)_           | _O(1)_           | _O(1)_       | according to the definition order of the enum values |

### **What is the Collection interface in Java?**

- The Collection interface is the interface which is implemented by all the classes in the collection framework. It declares the methods that every collection will have.

### **Which interfaces extend the Collection interface?**

- `List`
- `Set`
- `Queue`
- `Deque` (extends Queue, indirectly inherits Collection)

### **Is Map a part of the Collection interface hierarchy?**

No. `Map` is part of the Collections Framework but **does not extend** the `Collection` interface. It represents key-value pairs, not individual elements.

### **What are the commonly used methods of the Collection interface?**

| Method               | Description                       |
| -------------------- | --------------------------------- |
| `add(E e)`           | Adds an element                   |
| `remove(Object o)`   | Removes a specific element        |
| `clear()`            | Removes all elements              |
| `size()`             | Returns number of elements        |
| `isEmpty()`          | Checks if collection is empty     |
| `contains(Object o)` | Checks if element exists          |
| `iterator()`         | Returns an iterator for traversal |

### **What is the difference between Collection and Collections?**

- `Collection` is an **interface**.
- `Collections` is a **utility class** with static methods like `sort()`, `shuffle()`, `reverse()`.

### **What is the purpose of the `iterator()` method?**

It returns an `Iterator` object used to traverse the elements of the collection one by one.

```java
Iterator<String> it = collection.iterator(); 
while (it.hasNext()) {
	System.out.println(it.next());
}
```

### **What happens if you modify a collection while iterating?**

You may get a `ConcurrentModificationException`. To safely remove elements during iteration, use `Iterator.remove()` method.

### **What is the return type of `add()` method in Collection interface?**

It returns a `boolean`. `true` if the element was added successfully.

### **What is the use of `removeIf()` method in Collection?**

It removes all elements that match a given condition (predicate).

```java
list.removeIf(n -> n % 2 == 0); // remove even numbers
```

### **What is the default implementation of Collection?**

There is **no default implementation** of `Collection`. You need to use a class like `ArrayList`, `HashSet`, etc.

### **Is Collection thread-safe?**

No. Implementations like `ArrayList` or `HashSet` are **not thread-safe**. You can use:

- `Collections.synchronizedCollection(collection)`
- `CopyOnWriteArrayList` for thread-safe operations

### **Can we sort a Collection?**

Only if it's a `List`, using `Collections.sort()`. Other collections like `Set` don't support sorting directly.

### **What’s the difference between `add()` and `addAll()`?**

- `add()` inserts a single element
- `addAll()` adds all elements from another collection

### **How do you convert a Collection to an array?**

Using the `toArray()` method.

```java
Object[] array = collection.toArray();
```

### **What happens when we call `equals()` on two collections?**

They are considered equal if:
- They have the same size
- They contain the same elements (in the same order, if it's a List)

### **Does the Collection interface allow null values?**

Yes, but it depends on the implementation:
- `ArrayList` and `HashSet` allow null
- `TreeSet` may throw `NullPointerException` due to sorting logic

### **What does `retainAll()` method do in Collection?**

It retains only the elements that are also present in another collection (intersection).

```java
collection1.retainAll(collection2);
```

---

### **Explain the Java ListIterator Interface**

ListIterator Interface is used to traverse the element in a backward and forward direction.

Methods of Java ListIterator Interface:

| Method                | Description                                                                                                          |
| --------------------- | -------------------------------------------------------------------------------------------------------------------- |
| void add(E e)         | This method inserts the specified element into the list.                                                             |
| boolean hasNext()     | This method returns true if the list iterator has more elements while traversing the list in the forward direction.  |
| E next()              | This method returns the next element in the list and advances the cursor position.                                   |
| int nextIndex()       | This method returns the index of the element that would be returned by a subsequent call to next()                   |
| boolean hasPrevious() | This method returns true if this list iterator has more elements while traversing the list in the reverse direction. |
| E previous()          | This method returns the previous element in the list and moves the cursor position backward.                         |
| E previousIndex()     | This method returns the index of the element that would be returned by a subsequent call to previous().              |
| void remove()         | This method removes the last element from the list that was returned by next() or previous() methods                 |
| void set(E e)         | This method replaces the last element returned by next() or previous() methods with the specified element.           |

### **When to use what?**

![[attachments/Pasted image 20240525052708.png]]

![[attachments/Pasted image 20240525053052.png]]
