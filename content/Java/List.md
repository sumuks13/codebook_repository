tags: [[Java/Collection]], [[Data Structures/Data Structures]]

##### **List Interface**

- List interface is the child interface of Collection interface. 
- It inhibits a list type data structure in which we can store the **ordered** collection of objects. 
- It can have **duplicate** values.
- List interface is implemented by the classes ArrayList, LinkedList, Vector, and Stack.

##### **ArrayList**

- The ArrayList class implements the List interface. 
- It uses a dynamic array to store the duplicate element of different data types. 
- The ArrayList class maintains the insertion order and is **non-synchronized**.
- Capacity represents the total number of elements the array list can contain. 
- Therefore, the capacity of an array list is always greater than or equal to the size of the array list.
- When we add an element to the array list, it checks whether the size of the array list has become equal to the capacity or not. If yes, then the capacity of the array list increases.

```java
//Converting ArrayList to Array  
String[] array = fruitList.toArray(new String[fruitList.size()]);
String[] array = fruitList.toArray();
```

##### **LinkedList**

- LinkedList implements the Collection interface. 
- It uses a doubly linked list internally to store the elements. 
- It can store the duplicate elements. It maintains the insertion order and is not synchronized. 
- In LinkedList, the manipulation is fast because no shifting is required.

```java
//Traversing the list of elements in reverse order  
Iterator i=ll.descendingIterator();
```

##### **Vector**

- Vector uses a dynamic array to store the data elements. 
- It is similar to ArrayList. However, It is **<mark style="background: #FFF3A3A6;">synchronized</mark>** and contains many methods that are not the part of Collection framework.

##### **Stack**

The stack is the subclass of Vector. It implements the last-in-first-out data structure, i.e., Stack. The stack contains all of the methods of Vector class and also provides its methods like boolean push(), boolean peek(), boolean push(object o), which defines its properties.

#### **What is the List interface in Java?**

`List` is a subtype of `Collection` that represents an **ordered collection** of elements.  
It allows **duplicate elements** and **access by index**.

#### **What are the key features of List?**

- Maintains insertion order
- Allows duplicates
- Provides indexed access (`get(int index)`)
- Can contain null elements (based on implementation)

#### **Which classes implement the List interface?**

- `ArrayList`
- `LinkedList`
- `Vector`
- `Stack`

#### **What are the commonly used methods of List?**

|Method|Description|
|---|---|
|`add(E e)`|Adds element at end|
|`add(int index, E)`|Inserts at a specific position|
|`get(int index)`|Retrieves element at index|
|`remove(int index)`|Removes element at index|
|`set(int index, E)`|Replaces element|
|`indexOf(Object)`|Returns index of first occurrence|
|`lastIndexOf(Object)`|Returns index of last occurrence|
|`subList(from, to)`|Returns a view of a portion of the list|

#### **How is List different from Set?**

- **List**: Ordered, allows duplicates
- **Set**: Unordered (except LinkedHashSet), no duplicates

#### **How do you iterate over a List?**

Using:
- Enhanced for-loop
- Iterator
- For loop using index
- Java Streams

```java
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}
```

#### **What is the difference between ArrayList and LinkedList?**

|Feature|ArrayList|LinkedList|
|---|---|---|
|Backed by|Dynamic array|Doubly linked list|
|Access time|Fast (O(1) for `get`)|Slow (O(n) for `get`)|
|Insert/remove|Slow (shifting needed)|Fast (node manipulation)|
|Memory|Less (only data)|More (stores pointers)|

#### **Is List thread-safe?**

No. Implementations like `ArrayList` and `LinkedList` are not thread-safe.  
Use:
- `Collections.synchronizedList(new ArrayList<>())`
- `CopyOnWriteArrayList` for concurrent scenarios

#### **What is the default capacity of an ArrayList?**

Default capacity is **10**, and it grows by 50% when full (in Java 8+).

#### **Can a List contain null values?**

Yes. Most implementations like `ArrayList` and `LinkedList` allow null elements.

#### **How does `ArrayList` grow internally?**

It creates a new array with a larger capacity, copies existing elements, and replaces the old array.

#### **What happens when you access an invalid index in a List?**

You get an `IndexOutOfBoundsException`.

#### **How can you remove all elements from a List?**

Using `clear()` method.

```java
list.clear();
```

#### **What is the time complexity of operations in List?**

| Operation       | ArrayList        | LinkedList |
| --------------- | ---------------- | ---------- |
| `get()`         | O(1)             | O(n)       |
| `add()` (end)   | O(1) (amortized) | O(1)       |
| `add()` (index) | O(n)             | O(n)       |
| `remove()`      | O(n)             | O(n)       |

#### **How to convert an array to a List?**

```java
List<String> list = Arrays.asList("A", "B", "C");
```

#### **How to sort a List?**

```java
Collections.sort(list); // natural order
```

You can also use a custom comparator.

#### **What is the difference between `add()` and `set()`?**

- `add()` inserts a new element
- `set()` replaces an existing one at the specified index

#### **What is `List.of()` in Java 9?**

It creates an immutable list.

```java
List<String> list = List.of("A", "B", "C");
```

Modifying it will throw `UnsupportedOperationException`.

#### **What does `subList()` do?**

It returns a view of a specific range of the list. Changes made will also reflect in the original list.

#### **How to remove duplicates from a List?**

```java
List<String> unique = new ArrayList<>(new HashSet<>(list));
```