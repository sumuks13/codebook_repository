---
{"publish":true,"title":"Collections","cssclasses":""}
---

## Definitions

**Collection**: Root interface for group of objects. Types: List (ordered), Set (unique), Queue (FIFO), Map (key-value).

**List**: Ordered collection allowing duplicates and null. Index-based access. Implementations: ArrayList (dynamic array), LinkedList (doubly-linked).

**Set**: Unordered collection disallowing duplicates. No index access. Implementations: HashSet (hash-based), TreeSet (sorted, Red-Black tree).

**Map**: Key-value pairs with unique keys. Implementations: HashMap (fast, unordered), TreeMap (sorted keys), LinkedHashMap (insertion order).

**Iterator**: Object for traversing collections. Provides hasNext(), next(), remove() methods.

---

## QnA

**What's the difference between List and Set?**

Lists are ordered collections that allow duplicates and provide index-based access (e.g., `get(0)` returns first element). Sets are unordered collections that disallow duplicates and don't provide index-based access. If you need to maintain order and allow duplicates, use List; if you need uniqueness with fast lookup, use Set.

**When should you use ArrayList vs LinkedList?**

ArrayList provides fast random access with O(1) complexity but slow insertion/deletion at the middle (O(n)) because elements need shifting. LinkedList has slow access (O(n)) but fast insertion/deletion at any position (O(1)) since it only requires pointer updates. Use ArrayList for frequent access; use LinkedList for frequent insertions/deletions.

**Why doesn't Map extend Collection?**

Maps have key-value pair structure fundamentally different from Collections which hold single values. The contract and iteration pattern differ: Collection iterates values, while Map iterates entries (key-value pairs). This structural difference warranted a separate interface hierarchy.

**What's the difference between HashMap and TreeMap?**

| Feature | HashMap | TreeMap |
|---------|---------|---------|
| Performance | O(1) average case | O(log n) |
| Ordering | Unordered (no predictable order) | Sorted by key (uses Comparator) |
| Null Keys | Allows one null key | No null keys allowed |
| Memory | Lower memory overhead | Slightly higher due to tree structure |
| Use Case | Fast lookup when order doesn't matter | Need sorted keys or range queries |

**Can HashMap have null keys and values?**

Yes, HashMap allows one null key and multiple null values. This is a key difference from TreeMap, which doesn't allow null keys because the `compareTo()` method would throw NullPointerException. However, null values are allowed in TreeMap.

**What's the default capacity of ArrayList?**

ArrayList starts with a default capacity of 10 elements. When the list becomes full, it grows by 50% (new size = oldSize * 3/2). This dynamic growth strategy balances memory efficiency with performance, avoiding excessive copying operations.

---

## Code Examples

**Example - List Operations:**
```java
List<String> fruits = new ArrayList<>();
fruits.add("Apple");
fruits.add("Banana");
fruits.add("Orange");

// Access by index
System.out.println(fruits.get(0));  // Apple

// Iteration
for (String fruit : fruits) {
    System.out.println(fruit);
}

// Using Iterator
Iterator<String> it = fruits.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}

// Remove
fruits.remove(1);  // Remove by index
fruits.remove("Banana");  // Remove by value
```

**Example - Set Operations:**
```java
Set<Integer> numbers = new HashSet<>();
numbers.add(10);
numbers.add(20);
numbers.add(10);  // Duplicate, ignored
System.out.println(numbers.size());  // 2

// Order not guaranteed
for (Integer num : numbers) {
    System.out.println(num);
}

// TreeSet maintains sorted order
Set<Integer> sorted = new TreeSet<>(numbers);
for (Integer num : sorted) {
    System.out.println(num);  // 10, 20 (sorted)
}
```

**Example - Map Operations:**
```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 95);
scores.put("Bob", 87);
scores.put("Charlie", 92);

// Get value
System.out.println(scores.get("Alice"));  // 95

// Check containment
if (scores.containsKey("Alice")) {
    System.out.println("Alice found");
}

// Iterate entries
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// Get keys or values
for (String name : scores.keySet()) {
    System.out.println(name);
}

for (Integer score : scores.values()) {
    System.out.println(score);
}

// Remove
scores.remove("Bob");
```

**Example - Collections Utility Methods:**
```java
List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 1, 5));

Collections.sort(list);              // Ascending order
Collections.reverse(list);           // Reverse
Collections.shuffle(list);           // Random order
int maxVal = Collections.max(list);  // Maximum element
Collections.fill(list, 0);           // Fill with value
Collections.replaceAll(list, 1, 99); // Replace all occurrences

// Unmodifiable collections
List<Integer> immutable = Collections.unmodifiableList(list);
```

---

## Key Points

- **Fail-Fast Iterator**: If collection modified during iteration, ConcurrentModificationException thrown. Use Iterator.remove() for safe removal.
- **HashMap Collision**: Uses chaining (linked list) for hash collisions. Java 8+ uses balanced trees when chain length exceeds 8.
- **Collection Ordering**: List maintains insertion order; HashSet doesn't; TreeSet sorted; LinkedHashSet preserves insertion order.
- **Performance Trade-off**: HashMap faster but unordered; TreeMap slower but sorted—choose based on need.
- **Thread Safety**: Collections not thread-safe by default. Use `Collections.synchronizedList()`, `ConcurrentHashMap`, or external synchronization.
