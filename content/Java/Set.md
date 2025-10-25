tags: [[Java/Collection]], [[Data Structures/Data Structures]]


### **Set Interface**

- Set Interface in Java is present in java.util package. 
- It extends the Collection interface. 
- It represents the unordered set of elements which doesn't allow us to store the duplicate items. We can store at most one null value in Set. 
- Set is implemented by HashSet, LinkedHashSet, and TreeSet.

### **What is the Set interface in Java?**

- `Set` represents a group of **unique elements**.  
- It **does not allow duplicates** and may or may not maintain insertion order based on the implementation.

### **What are the characteristics of a Set?**

- No duplicate elements allowed
- At most one `null` element (implementation dependent)
- Order is not guaranteed (unless using `LinkedHashSet` or `TreeSet`)

### **Which classes implement the Set interface?**

- `HashSet`
- `LinkedHashSet`
- `TreeSet`
- `EnumSet`
- `CopyOnWriteArraySet`

### **What is the difference between Set and List?**

| Feature    | List                        | Set                              |
| ---------- | --------------------------- | -------------------------------- |
| Duplicates | Allowed                     | Not allowed                      |
| Order      | Maintains insertion order   | Unordered (except LinkedHashSet) |
| Indexing   | Supports index-based access | Not supported                    |

### **What is a HashSet?**

- An implementation of `Set` backed by a `HashMap`
- Does not maintain insertion order
- Allows one `null` value
- Offers constant time performance for basic operations like `add()`, `remove()`, and `contains()`

### **What is a LinkedHashSet?**

- Subclass of `HashSet`
- Maintains **insertion order**
- Slightly slower than `HashSet` due to extra memory usage

### **What is a TreeSet?**

- Implements `NavigableSet`
- Stores elements in **sorted (natural or custom) order**
- Does **not allow null**
- Backed by a Red-Black tree

### **When should you use HashSet, LinkedHashSet, or TreeSet?**

| Use Case                     | Preferred Set Type |
| ---------------------------- | ------------------ |
| Fast access, no order needed | `HashSet`          |
| Maintain insertion order     | `LinkedHashSet`    |
| Need sorted elements         | `TreeSet`          |

### **What is the SortedSet Interface ?** 

- SortedSet is the alternate of Set interface that provides a total ordering on its elements. 
- The elements of the SortedSet are arranged in the increasing (ascending) order.

### **How does Set ensure uniqueness?**

Through `equals()` and `hashCode()` methods for `HashSet`/`LinkedHashSet`, and `compareTo()` or `Comparator` for `TreeSet`.

### **What happens if you add a duplicate to a Set?**

The duplicate is ignored. The `add()` method returns `false`.

### **Can you store null in a Set?**

Yes, but only one null is allowed in `HashSet` and `LinkedHashSet`.  
`TreeSet` does not allow `null` if using natural ordering.

### **Explain the ClassCast Exception in TreeSet**

If we add an object of the class that is not implementing the Comparable interface, the ClassCast Exception is raised.

It is because the TreeSet maintains the sorting order, and for doing the sorting the comparison of different objects that are being inserted in the TreeSet is must, which is accomplished by implementing the Comparable interface. So all the Objects used for TreeSet must implement Comparable interface.

### **How do you sort a Set?**

- Convert it to a `List` and sort
- Use a `TreeSet` for automatic sorting

```java
Set<String> sortedSet = new TreeSet<>(unsortedSet);
```

### **What is the time complexity of Set operations?**

|Operation|HashSet|LinkedHashSet|TreeSet|
|---|---|---|---|
|add|O(1)|O(1)|O(log n)|
|remove|O(1)|O(1)|O(log n)|
|contains|O(1)|O(1)|O(log n)|

### **What is EnumSet?**

- Specialized Set implementation for use with enum types only
- Very fast and memory-efficient
- Cannot store `null`

```java
enum Day { MON, TUE, WED }
Set<Day> days = EnumSet.of(Day.MON, Day.WED);
```

### **What is CopyOnWriteArraySet?**

- A thread-safe Set backed by a `CopyOnWriteArrayList`
- Ideal for frequent iteration and less frequent updates
- Used in concurrent applications

### **Can a Set be synchronized?**

Yes:

```java
Set<String> syncSet = Collections.synchronizedSet(new HashSet<>());
```

### **How to remove duplicates from a List using Set?**

```java
List<String> list = Arrays.asList("A", "B", "A");
Set<String> set = new HashSet<>(list);
```

### **What is the output if we insert elements with the same hashcode into a HashSet?**

- Elements with the same hashcode are stored in the same bucket (linked list or tree)
- Uniqueness is still checked using `equals()`

### **How to compare two Sets for equality?**

Use `.equals()` method. It returns `true` if both sets contain the same elements, regardless of order.

### **How do you convert a Set to an array?**

```java
String[] array = set.toArray(new String[0]);
```

### **How do you remove elements conditionally in a Set?**

```java
set.removeIf(s -> s.length() > 3);
```

### **What does `retainAll()` do in Set?**

It performs set **intersection** — keeps only elements also present in the given collection.

```java
set1.retainAll(set2);
```
