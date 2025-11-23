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
