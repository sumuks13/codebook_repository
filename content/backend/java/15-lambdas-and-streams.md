---
{"publish":true,"title":"Lambdas & Stream","cssclasses":""}
---

## Definitions

**Functional Interface**: Interface with exactly one abstract method. Target for lambda expressions and method references. @FunctionalInterface annotation.

**Lambda Expression**: Anonymous function allowing concise syntax for simple implementations. Syntax: `(parameters) -> expression/body`.

**Method Reference**: Shorthand referencing existing methods. Syntax: `ClassName::methodName` or `instance::methodName`.

**Stream API**: Enables functional-style processing of collections. Lazy evaluation; supports filter, map, reduce operations.

**Higher-Order Function**: Function taking function(s) as parameter or returning a function.

**Functional Composition**: Combining multiple functions into single function for complex transformations.

---

## QnA

**What's a functional interface?**

A functional interface has exactly one abstract method, enabling lambda expressions for implementation. Can have multiple default or static methods. The @FunctionalInterface annotation (optional) documents intent and causes compile error if multiple abstract methods added. Examples: Runnable, Callable, Comparator, and custom interfaces with single abstract method.

**What are the core functional interfaces in java.util.function?**

Key built-in interfaces include: Predicate (takes input, returns boolean for testing conditions), Consumer (accepts input with no return), Supplier (no input, returns value), and Function (transforms input to output). These four form the foundation; others like BiFunction extend with multiple parameters. Understanding these enables elegant functional code.

**What's type inference in lambda?**

Compiler infers parameter and return types from context (the target functional interface). For example, `(a, b) -> a + b` infers types from the function signature. Explicit type specification optional: `(int a, int b) -> a + b`. Single-parameter lambdas can omit parentheses: `x -> x * 2`. Type inference makes lambdas concise while maintaining type safety.

**What's the difference between Stream and Collection?**

Collections are data structures storing elements; Streams are pipelines processing them lazily. Collections can be reused; Streams are consumed after terminal operation. Collections store everything in memory; Streams process one element at a time. Collections are imperative (how); Streams are functional (what).

**Are stream operations lazy?**

Intermediate operations (map, filter) are lazy—they don't execute until a terminal operation triggers evaluation. Terminal operations (forEach, collect) force evaluation. This enables optimizations like early termination and prevents unnecessary processing. Lazy evaluation is powerful for infinite streams and large datasets.

**Can you reuse a stream?**

No, Streams are consumed after terminal operation. Attempting to reuse causes IllegalStateException. Create a new Stream for another operation. This is by design—Streams are meant for single-pass processing. If reuse needed, recreate from source collection or use iteration instead.

---

## Cheatsheet


**Creation (Getting a Stream)**

| **Method**                                   | **Description**                                                    | **Example**                     |
| -------------------------------------------- | ------------------------------------------------------------------ | ------------------------------- |
| **`Collection.stream()`**                    | Creates a stream from a Collection (List, Set, etc.).              | `list.stream()`                 |
| **`Arrays.stream(T[] array)`**               | Creates a stream from an array.                                    | `Arrays.stream(names)`          |
| **`Stream.of(T... values)`**                 | Creates a stream with a fixed number of elements.                  | `Stream.of("a", "b", "c")`      |
| **`IntStream/LongStream.range(start, end)`** | Creates a stream of primitive integers/longs (exclusive end).      | `IntStream.range(1, 10)`        |
| **`Stream.generate(...)`**                   | Creates an **infinite** stream using a `Supplier`.                 | `Stream.generate(Math::random)` |
| **`Stream.iterate(...)`**                    | Creates an **infinite** stream by iteratively applying a function. | `Stream.iterate(0, n -> n + 2)` |

**Intermediate Operations (Lazy & Chainable)**: Intermediate operations transform the stream and return a new stream. They are **lazy** and execute only when a terminal operation is called.

| **Operation**    | **Function**             | **Description**                                                   | **Example**                                        |
| ---------------- | ------------------------ | ----------------------------------------------------------------- | -------------------------------------------------- |
| **`filter()`**   | `Predicate<T>`           | Selects elements that match a condition.                          | `.filter(n -> n > 5)`                              |
| **`map()`**      | `Function<T, R>`         | Transforms each element into a new object/type.                   | `.map(String::toUpperCase)`                        |
| **`flatMap()`**  | `Function<T, Stream<R>>` | **Flattens** a stream of collections/arrays into a single stream. | `.flatMap(List::stream)`                           |
| **`distinct()`** | None                     | Removes duplicate elements based on `equals()`.                   | `.distinct()`                                      |
| **`sorted()`**   | `Comparator<T>`          | Sorts elements. Takes an optional `Comparator`.                   | `.sorted()` / `.sorted(Comparator.reverseOrder())` |
| **`peek()`**     | `Consumer<T>`            | Performs an action on each element, useful for debugging/logging. | `.peek(System.out::println)`                       |
| **`limit()`**    | `long maxSize`           | Short-circuits the stream after the given number of elements.     | `.limit(10)`                                       |
| **`skip()`**     | `long n`                 | Skips the first $N$ elements of the stream.                       | `.skip(5)`                                         |

**Primitive Stream Mapping**: These operations convert a stream of wrapper objects (`Stream<Integer>`, `Stream<Double>`, etc.) to a specialized, efficient primitive stream.

| **Conversion**      | **Purpose**       | **Example**                       |
| ------------------- | ----------------- | --------------------------------- |
| **`mapToInt()`**    | To `IntStream`    | `.mapToInt(String::length)`       |
| **`mapToLong()`**   | To `LongStream`   | `.mapToLong(MyObject::getId)`     |
| **`mapToDouble()`** | To `DoubleStream` | `.mapToDouble(Product::getPrice)` |

**Terminal Operations (Eager & Final)**: Terminal operations start the stream pipeline, produce a result, and consume the stream.

|**Operation**|**Return Type**|**Description**|**Example**|
|---|---|---|---|
|**`forEach()`**|`void`|Executes an action for each element. (Non-stream result)|`.forEach(System.out::println)`|
|**`collect()`**|`R` (flexible)|Accumulates elements into a mutable container (List, Set, Map, etc.).|`.collect(Collectors.toList())`|
|**`toList()`** (Java 16+)|`List<T>`|Simple collection shortcut.|`.toList()`|
|**`reduce()`**|`T` or `Optional<T>`|Combines elements into a single result using an accumulator function.|`.reduce(1, (a, b) -> a * b)`|
|**`count()`**|`long`|Returns the number of elements in the stream.|`.count()`|
|**`min() / max()`**|`Optional<T>`|Returns the min/max element based on a `Comparator`.|`.max(Comparator.naturalOrder())`|
|**`findFirst()`**|`Optional<T>`|Returns the first element (short-circuiting).|`.findFirst()`|
|**`findAny()`**|`Optional<T>`|Returns any element (useful for parallel streams).|`.findAny()`|
|**`allMatch()`**|`boolean`|Checks if **all** elements match the `Predicate`.|`.allMatch(n -> n > 0)`|
|**`anyMatch()`**|`boolean`|Checks if **any** element matches the `Predicate`.|`.anyMatch(n -> n == 0)`|
|**`noneMatch()`**|`boolean`|Checks if **no** elements match the `Predicate`.|`.noneMatch(n -> n < 0)`|

Collectors (`collect()`): The `Collectors` utility class provides factory methods for common reduction operations.

|**Collector Method**|**Return Type**|**Description**|**Example**|
|---|---|---|---|
|**`toList()`**|`List<T>`|Collects elements into a `List`.|`.collect(Collectors.toList())`|
|**`toSet()`**|`Set<T>`|Collects elements into a `Set`.|`.collect(Collectors.toSet())`|
|**`joining()`**|`String`|Concatenates strings with an optional delimiter.|`.collect(joining(", "))`|
|**`counting()`**|`Long`|Counts the elements.|`.collect(counting())`|
|**`summingInt()`**|`Integer`|Sums elements after applying an `IntMapper`.|`.collect(summingInt(Product::getQuantity))`|
|**`averagingDouble()`**|`Double`|Calculates the average.|`.collect(averagingDouble(Person::getAge))`|
|**`groupingBy()`**|`Map<K, List<T>>`|Groups elements by a key function.|`.collect(groupingBy(Book::getAuthor))`|
|**`partitioningBy()`**|`Map<Boolean, List<T>>`|Splits elements into two groups (true/false) based on a `Predicate`.|`.collect(partitioningBy(User::isActive))`|
|**`mapping()`**|(Downstream)|Transforms elements **within** a grouping collector.|`.collect(groupingBy(..), mapping(..))`|

Reduction (`reduce()` vs. Primitive Methods)

|**Operation**|**Signature**|**Benefit**|**Use Case**|
|---|---|---|---|
|**`reduce(identity, accumulator)`**|`T`|Most flexible; can perform any accumulation.|Product: `.reduce(1, (a, b) -> a * b)`|
|**`sum()`** (on primitive streams)|`int/long/double`|Highly efficient and returns the primitive result directly.|Sum of prices: `.mapToInt(p -> p).sum()`|
|**`average()`** (on primitive streams)|`OptionalDouble`|Handles empty streams gracefully and is efficient.|`.mapToDouble(d -> d).average()`|

---

## Code Examples

**Example - Functional Interfaces:**
```java
Predicate<Integer> isEven = n -> n % 2 == 0;
System.out.println(isEven.test(4));  // true

Consumer<String> printer = System.out::println;
printer.accept("Hello");

Supplier<String> supplier = () -> "Hello";
String value = supplier.get();

Function<Integer, Integer> square = x -> x * x;
System.out.println(square.apply(5));  // 25
```

**Example - Lambda Expressions:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Traditional anonymous class
List<Integer> evens = new ArrayList<>();
for (Integer n : numbers) {
    if (n % 2 == 0) {
        evens.add(n);
    }
}

// Lambda approach (cleaner)
List<Integer> evensLambda = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
```

**Example - Method References:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Lambda
names.forEach(name -> System.out.println(name));

// Method reference (cleaner)
names.forEach(System.out::println);

// Equivalent forms
Function<String, Integer> strLen = String::length;
Function<String, Integer> strLen2 = s -> s.length();
```

**Example - Stream Operations:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Filter, map, reduce
int sum = numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 2)
    .reduce(0, Integer::sum);

// Collectors
List<Integer> doubled = numbers.stream()
    .map(n -> n * 2)
    .collect(Collectors.toList());

// Parallel streams
int parallelSum = numbers.parallelStream()
    .reduce(0, Integer::sum);
```

---

## Key Points

- **Immutability**: Lambda captures variables must be effectively final (not modified after capture).
- **Lazy Evaluation**: Intermediate operations don't execute until terminal operation; enables optimization.
- **Stream Characteristics**: Stateless operations preferred; avoid external state modification within streams.
- **Performance**: Stream overhead minimal for small collections; benefits large datasets or complex chains.
- **Method Reference Types**: Static (`Class::method`), instance (`obj::method`), constructor (`Class::new`).
- For numerical calculations, use **primitive streams** (`IntStream`, `DoubleStream`, or `LongStream`) over generic `Stream<T>` for **optimized terminal operations** like `sum()`, `average()`, and `max()`.
