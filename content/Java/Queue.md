tags: [[Java/Collection]], [[Data Structures/Data Structures]]

### **What is the Queue interface in Java?**

- `Queue` is a subinterface of `Collection` that represents a **FIFO (First-In-First-Out)** data structure.  
- It is used for **holding elements before processing** them.

### **What are the key features of Queue?**

- Follows FIFO order (unless it’s a `PriorityQueue`)
- Supports adding at the tail and removing from the head
- Often used in scenarios like **task scheduling**, **order processing**, and **message queues**

### **Which classes implement the Queue interface?**

- `LinkedList`
- `PriorityQueue`
- `ArrayDeque`
- `ConcurrentLinkedQueue`
- `LinkedBlockingQueue`
- `DelayQueue` (from `java.util.concurrent`)

### **What are the common methods in the Queue interface?**

| Method       | Description                                              |
| ------------ | -------------------------------------------------------- |
| `add(E e)`   | Adds element to the queue, throws exception if fails     |
| `offer(E e)` | Adds element, returns `false` if fails                   |
| `remove()`   | Removes and returns the head, throws exception if empty  |
| `poll()`     | Removes and returns head, returns `null` if empty        |
| `element()`  | Returns head without removing, throws exception if empty |
| `peek()`     | Returns head without removing, returns `null` if empty   |

### **Which implementation of Queue should you use for general-purpose use?**

- `LinkedList` if you need a simple Queue (it implements both `List` and `Queue`)
- `ArrayDeque` for efficient non-blocking FIFO/stack operations
- `PriorityQueue` for sorted elements
- `ConcurrentLinkedQueue` for thread-safe operations

### **What is PriorityQueue?**

A `PriorityQueue` is a queue where **elements are ordered based on priority**.  
It does **not follow FIFO**; the element with the **highest priority (lowest value)** comes out first.

```java
Queue<Integer> pq = new PriorityQueue<>();
pq.add(10);
pq.add(5);
pq.add(20);
poll() //returns 5
```

### **Does PriorityQueue allow null values?**

No. `PriorityQueue` does not allow `null` elements.  
Adding `null` will throw `NullPointerException`.

### **Is Queue thread-safe?**

- Basic implementations like `LinkedList`, `ArrayDeque` and `PriorityQueue` are **not thread-safe**.
- Use `ConcurrentLinkedQueue`, `LinkedBlockingQueue`, or other concurrent implementations for thread safety.

### **What is Deque in Java?**

`Deque` stands for **Double-Ended Queue**.  
It allows elements to be **added or removed from both ends**.

Implementations:
- `ArrayDeque` (non-thread-safe, efficient)
- `LinkedBlockingDeque` (thread-safe)

### **What is the difference between Queue and Deque?**

| Feature   | Queue      | Deque                              |
| --------- | ---------- | ---------------------------------- |
| Insertion | End only   | Both ends                          |
| Removal   | Front only | Both ends                          |
| Use Cases | FIFO tasks | Stacks, deques, double-ended logic |

### **What is ArrayDeque?**

- `ArrayDeque` is a resizable array implementation of `Deque`.  
- It is **faster than `Stack` and `LinkedList`** for stack/queue operations.  
- It does **not allow null elements**.

### **How to implement a stack using Queue or Deque?**

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10); // addFirst()
stack.pop();    // removeFirst()
```

### **What is BlockingQueue?**

`BlockingQueue` is an interface that supports **blocking operations** for producers and consumers.  
- If the queue is empty, `take()` will wait.  
- If the queue is full, `put()` will wait.

### **Which classes implement BlockingQueue?**

- `LinkedBlockingQueue`
- `ArrayBlockingQueue`
- `PriorityBlockingQueue`
- `DelayQueue`

### **What is the difference between ConcurrentLinkedQueue and LinkedBlockingQueue?**

| Feature          | ConcurrentLinkedQueue          | LinkedBlockingQueue |
| ---------------- | ------------------------------ | ------------------- |
| Blocking support | No                             | Yes                 |
| Bounded          | No                             | Yes (optionally)    |
| Use case         | Non-blocking, high concurrency | Producer-consumer   |

### **How to safely iterate a Queue?**

You can use an Iterator, but do not modify the queue structure during iteration.  
For concurrent use, prefer `ConcurrentLinkedQueue`.

### **Can you store null in a Queue?**

- **Not recommended** in most implementations
- `PriorityQueue`, `ArrayDeque`, and concurrent queues **do not allow null**

### **How to convert a Queue to a List?**

```java
Queue<String> queue = new LinkedList<>();
List<String> list = new ArrayList<>(queue);
```

### **When should you use a Queue in real applications?**

- **Task scheduling**
- **Order processing**
- **Message passing between threads**
- **Rate-limiting requests**