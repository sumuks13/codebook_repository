---
{"publish":true,"title":"Queue & Stack","cssclasses":""}
---

## Definitions

**Queue**: FIFO (First-In-First-Out) collection. Elements added at tail, removed from head. Implementations: LinkedList, PriorityQueue.

**Deque**: Double-ended queue allowing insertion/removal at both ends. Can be used as Queue or Stack.

**Stack**: LIFO (Last-In-First-Out) collection. Element added/removed from top. Legacy (Vector-based); use Deque for new code.

**PriorityQueue**: Elements ordered by priority (Comparator). Head is lowest-priority element.

**Optional**: Container for possibly null values. Avoids null checks; encourages functional approach.

---

## QnA

**What's the difference between Queue and Deque?**

Queue is a FIFO collection where elements are added at the tail and removed from the head. Deque (double-ended queue) allows adding and removing from both ends, making it flexible for both FIFO (queue) and LIFO (stack) operations. Choose Queue for strict FIFO semantics; Deque when you need flexibility.

**How does PriorityQueue order elements?**

PriorityQueue orders elements by natural order (using Comparable) or a custom Comparator. The head element is the smallest according to the ordering. It's implemented as a heap, providing O(log n) insertion/removal. Iterating through the queue doesn't produce sorted order; only removal guarantees order.

**Should you use the Stack class in new code?**

No, the Stack class extends Vector (legacy and synchronized). Use Deque with methods `push()`, `pop()`, `peek()` on ArrayDeque instead. ArrayDeque is faster, not synchronized, and provides better performance. For example, `Deque<Integer> stack = new ArrayDeque<>();`

**What methods does Queue provide?**

Queue provides three types of methods: add/offer (insert), remove/poll (remove), element/peek (inspect). Exception-throwing methods (add, remove, element) throw NoSuchElementException if queue is empty; null-returning methods (offer, poll, peek) return null. Use null-returning versions for safe handling.

**Why use Optional instead of null?**

Optional makes null handling explicit and allows functional chaining. Instead of checking `if (value != null)`, you chain operations like `value.map(...).orElse(default)`. This eliminates NullPointerException risks and encourages cleaner code. Optional is a container signaling a value might be absent.

**Can Optional be used in fields?**

Discouraged. Optional adds overhead and is designed for return types and method parameters. For fields, either allow null with clear documentation or use validation in constructors. Using Optional fields complicates code without significant benefit.

---

## Code Examples

**Example - Queue Operations:**
```java
Queue<String> queue = new LinkedList<>();
queue.add("Task1");
queue.add("Task2");
queue.add("Task3");

// Peek at head
System.out.println(queue.peek());  // Task1

// Remove from head
String task = queue.poll();        // Task1

// Iteration
while (!queue.isEmpty()) {
    System.out.println(queue.poll());
}

// Exception-throwing
Queue<Integer> q = new LinkedList<>();
// q.remove();  // NoSuchElementException
q.poll();      // Returns null (safe)
```

**Example - PriorityQueue (Min-Heap):**
```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.add(30);
minHeap.add(10);
minHeap.add(20);

System.out.println(minHeap.poll());  // 10
System.out.println(minHeap.poll());  // 20
System.out.println(minHeap.poll());  // 30

// Max-heap with reverse comparator
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
maxHeap.add(30);
maxHeap.add(10);
maxHeap.add(20);

System.out.println(maxHeap.poll());  // 30
```

**Example - Deque (Stack & Queue):**
```java
Deque<Integer> deque = new ArrayDeque<>();

// Queue operations
deque.addLast(1);
deque.addLast(2);
System.out.println(deque.pollFirst());  // 1

// Stack operations
deque.push(10);
deque.push(20);
System.out.println(deque.pop());  // 20

// Both ends access
System.out.println(deque.peekFirst());  // 10
System.out.println(deque.peekLast());   // 2
```

**Example - Optional Usage:**
```java
Optional<String> name = Optional.of("Alice");

// Check and process
if (name.isPresent()) {
    System.out.println(name.get());
}

// Use ifPresent (cleaner)
name.ifPresent(n -> System.out.println(n));

// Get with default
String value = name.orElse("Unknown");
value = name.orElseGet(() -> "Default");

// Transformations
Optional<Integer> length = name.map(String::length);
Optional<String> upper = name.filter(n -> n.startsWith("A")).map(String::toUpperCase);

// Empty optional
Optional<String> empty = Optional.empty();
System.out.println(empty.orElse("Default"));  // Default
```

---

## Key Points

- **Queue vs Deque**: Choose Queue for FIFO; Deque for flexibility (Stack/Queue behavior).
- **PriorityQueue Not Sorted**: Iterating doesn't give sorted order; only removal guarantees order.
- **ArrayDeque vs LinkedList**: ArrayDeque faster for Deque operations; LinkedList better for List needs.
- **Optional Chaining**: Use map(), flatMap(), filter() for functional transformations vs. multiple if-checks.
- **Exception vs Null**: Exception-throwing methods (add, remove, element) vs. null-returning (offer, poll, peek).
