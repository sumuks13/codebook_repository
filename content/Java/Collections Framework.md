tags: [[Java]], [[Collection]]

 **Collections framework** provides an architecture to store and manipulate group of objects.

### **How does the Java Collections hierarchy look?**

```
                Iterable
                   |
               Collection
            /      |      \
         List     Set     Queue
                           |
                         Deque
```

- `Map` is separate and not part of this hierarchy.

### **What is a framework in Java?**

- It provides readymade architecture.
- It represents a set of classes and interfaces.
- It is optional.

### **What is the Collections Framework in Java?**

Java Collections Framework is a **set of classes and interfaces** in the `java.util` package that provides **data structures** and **algorithms** to store, manipulate, and access groups of objects efficiently.

### **What are the main benefits of using the Collections Framework?**

- Reduces programming effort by using **ready-made data structures**
- Increases performance with **efficient implementations**
- Provides **interoperability** between different collection types
- Offers **algorithms** (like sorting, searching) and **utilities**

### **What is the difference between Collection and Collections?**

- `Collection`: an interface (root of the hierarchy)
- `Collections`: a utility class with **static methods** (e.g., `sort()`, `reverse()`, `shuffle()`)

### **How is Map different from other Collection types?**

`Map` is **not a subtype of Collection**.  
While `Collection` stores **single elements**, `Map` stores **key-value pairs**.

### **What is the role of Iterator in Collections?**

Iterator provides a **unified way to traverse** any collection.  
- It has methods:
	- `hasNext()`
	- `next()`
	- `remove()`

### **What is Iterable interface in Java?**

- `Iterable` is an **interface** in `java.lang` package.
- It represents a collection of elements that can be **iterated** (looped) one by one.
- It has **one method**:

```java
Iterator<T> iterator();
```

- All major collection classes like `ArrayList`, `HashSet`, etc., implement `Iterable`.
- It allows the use of **enhanced for loop** (`for-each`) in Java:

```java
List<String> list = List.of("A", "B", "C");
	for (String s : list) {
	    System.out.println(s); // works because List implements Iterable
}
```

### **What is Iterator interface in Java?**

- `Iterator` is an **interface** in `java.util` package.
- It is used to **traverse** or **iterate** over a collection **manually**.
- It has three main methods:

```java
boolean hasNext();      // checks if more elements exist
T next();               // returns the next element
void remove();          // removes the current element (less used)
```

Example:

```java
List<String> names = List.of("Alice", "Bob", "Charlie");

Iterator<String> it = names.iterator();

while (it.hasNext()) {
    String name = it.next();
    System.out.println(name);
}
```

### **What is the difference Between Iterable and Iterator**

|Feature|Iterable|Iterator|
|---|---|---|
|Type|Interface|Interface|
|Package|`java.lang`|`java.util`|
|Purpose|Represents a collection that can return an iterator|Used to iterate over elements|
|Key Method|`iterator()`|`hasNext()`, `next()`, `remove()`|
|Used In|`for-each` loop|`while` loop with `hasNext()`|
|Who Implements|`ArrayList`, `HashSet`, etc.|Implemented internally by collections|
### **What is the difference between Iterator and ListIterator?**

- `Iterator` works for all collections
- `ListIterator` works only for **List**
- `ListIterator` supports **bidirectional traversal** and **element modification**

### **What is the purpose of Comparable and Comparator?**

- `Comparable`: natural ordering using `compareTo()` (used in TreeSet, TreeMap)
- `Comparator`: custom ordering using `compare()` method

### **How do you sort a collection?**

Use `Collections.sort()` for a `List`:

```java
List<String> list = Arrays.asList("C", "A", "B");
Collections.sort(list);
```

For a custom order, use `Comparator`:

```java
Collections.sort(list, (a, b) -> b.compareTo(a)); // descending
```

### **What is the difference between fail-fast and fail-safe iterators?**

| Feature  | Fail-Fast                                          | Fail-Safe                                   |
| -------- | -------------------------------------------------- | ------------------------------------------- |
| Behavior | Throws `ConcurrentModificationException` on change | Works on a copy, no exception               |
| Examples | `ArrayList`, `HashSet`                             | `CopyOnWriteArrayList`, `ConcurrentHashMap` |

### **How to make a collection thread-safe?**

- Use **synchronized versions**:  
    `Collections.synchronizedList(new ArrayList<>())`
- Use **concurrent classes**:  
    `ConcurrentHashMap`, `CopyOnWriteArrayList`, etc.

### **What are some utility methods in the Collections class?**

- `sort()`, `reverse()`, `shuffle()`
- `min()`, `max()`
- `binarySearch()`
- `fill()`, `copy()`
- `synchronizedList()`, `unmodifiableList()`

### **What are some real-world use cases for the Collections Framework?**

- Storing form data or DB rows (List)
- Caching unique records (Set)
- Key-based lookup (Map)
- Message queues or task scheduling (Queue/Deque)
