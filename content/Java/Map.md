tags: [[Collection]], [[Data Structures]]

### **What is the Map interface in Java?**

`Map` is a part of the Java Collections Framework that stores **key-value pairs**.  
Each key is **unique**, and each key maps to **exactly one value**.

### **What are the key features of Map?**

- Stores data as **(key, value)** pairs
- Keys must be **unique**, but values can be **duplicated**
- Allows one `null` key and multiple `null` values (depending on implementation)

### **Which classes implement the Map interface?**

- `HashMap`
- `LinkedHashMap`
- `TreeMap`
- `Hashtable`
- `ConcurrentHashMap`
- `WeakHashMap`
- `EnumMap`

### **What is the difference between Map and Collection?**

- `Map` is **not a subtype** of `Collection`
- `Collection` stores elements; `Map` stores key-value pairs
- You can convert a `Map` to a `Set` or `Collection` via `keySet()`, `values()`, or `entrySet()`

### **What are the main methods of the Map interface?**

| Method                    | Description                        |
| ------------------------- | ---------------------------------- |
| `put(K key, V value)`     | Adds or replaces a key-value pair  |
| `get(Object key)`         | Returns value for the key          |
| `remove(Object key)`      | Removes entry for the key          |
| `containsKey(Object key)` | Checks if key exists               |
| `containsValue(Object v)` | Checks if value exists             |
| `keySet()`                | Returns a Set of all keys          |
| `values()`                | Returns a Collection of all values |
| `entrySet()`              | Returns Set of key-value entries   |

### **What is HashMap in Java?**

`HashMap` is a commonly used implementation of `Map` backed by a **hash table**.  
- It allows **one null key** and **multiple null values**.  
- It **does not maintain order**.

### **What is LinkedHashMap?**

A subclass of `HashMap` that maintains **insertion order** of entries.  
Useful when you want predictable iteration order.

### **What is TreeMap?**

A `Map` implementation that keeps keys in **sorted (natural or custom) order**.  
It is based on a **Red-Black tree** and **does not allow null keys**.

### **What is the difference between HashMap, LinkedHashMap, and TreeMap?**

|Feature|HashMap|LinkedHashMap|TreeMap|
|---|---|---|---|
|Order|No order|Insertion order|Sorted order|
|Null key allowed|Yes (1)|Yes (1)|No|
|Performance|Fast|Slightly slower|Slower (log n)|

---

### **Is Map thread-safe?**

- `HashMap`, `LinkedHashMap`, and `TreeMap` are **not thread-safe**
    
- Use `ConcurrentHashMap` for concurrent access
    
- `Collections.synchronizedMap(new HashMap<>())` adds synchronization
    

---

### **What is ConcurrentHashMap?**

A thread-safe version of `HashMap` that allows **concurrent reads and updates** without locking the entire map.  
Ideal for multi-threaded applications.

---

### **What is Hashtable?**

An older synchronized version of `Map`.  
It’s thread-safe but **less efficient** than `ConcurrentHashMap`.

---

### **Can Map store null keys or values?**

|Implementation|Null Key|Null Value|
|---|---|---|
|`HashMap`|Yes (1)|Yes (many)|
|`LinkedHashMap`|Yes (1)|Yes|
|`TreeMap`|No|Yes|
|`ConcurrentHashMap`|No|No|
|`Hashtable`|No|No|

---

### **How do you iterate over a Map?**

There are 3 common ways:

```java
// Using entrySet
for (Map.Entry<K, V> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " = " + entry.getValue());
}

// Using keySet
for (K key : map.keySet()) {
    System.out.println(key + " = " + map.get(key));
}

// Using forEach (Java 8+)
map.forEach((k, v) -> System.out.println(k + " = " + v));
```

---

### **How does HashMap handle collisions?**

It uses a **bucket-based structure** where collisions are handled by:

- **Linked list chaining** (before Java 8)
    
- **Tree (red-black tree) + Linked list** (after threshold in Java 8)
    

---

### **What happens when you insert a duplicate key into a Map?**

The new value **replaces** the old one for the same key.

```java
map.put("A", 1);
map.put("A", 2); // now "A" maps to 2
```

---

### **How to sort a Map by keys?**

```java
Map<String, Integer> sortedMap = new TreeMap<>(map);
```

---

### **How to sort a Map by values?**

```java
map.entrySet()
   .stream()
   .sorted(Map.Entry.comparingByValue())
   .forEach(System.out::println);
```

---

### **How to check if a Map is empty or get its size?**

```java
map.isEmpty();   // returns true if map is empty
map.size();      // returns number of entries
```

---

### **What is the purpose of entrySet(), keySet(), and values()?**

|Method|Description|
|---|---|
|`entrySet()`|Set of key-value pairs (`Map.Entry`)|
|`keySet()`|Set of keys|
|`values()`|Collection of all values|

---

### **What is WeakHashMap?**

A `Map` that stores keys as **weak references**.  
When a key is no longer referenced outside the map, it is automatically garbage collected.

---

### **What is EnumMap?**

A high-performance `Map` specifically for **enum keys**.  
It is compact and very fast but does **not allow null keys**.

---

### **Can you remove entries while iterating a Map?**

Yes, using the `Iterator` from `entrySet()`:

```java
Iterator<Map.Entry<K, V>> it = map.entrySet().iterator();
while (it.hasNext()) {
    Map.Entry<K, V> entry = it.next();
    if (entry.getValue() < 0) {
        it.remove();
    }
}
```

---

### **What are some real-world use cases of Map?**

- Caching data (e.g., user sessions, config settings)
    
- Grouping and counting items
    
- Lookup tables
    
- Storing key-value structured data like JSON
    

---

Ready to move to the next topic — **Iterator & Iterable**?