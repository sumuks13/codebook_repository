---
{"publish":true,"title":"Generics","cssclasses":""}
---

## Definitions

**Generics**: Feature enabling type-safe collections and classes working with multiple types. Type parameters specified at compile-time, type-checked before runtime.

**Type Parameter**: Placeholder for actual type (e.g., `<T>`, `<E>`, `<K, V>`). Single letters by convention: T=Type, E=Element, K=Key, V=Value.

**Bounded Type Parameter**: Restricts type parameter to specific type or subtypes. Syntax: `<T extends ClassName>`.

**Wildcard**: `?` represents unknown type. Used in method parameters/returns for flexibility. Types: `? extends` (upper bound), `? super` (lower bound).

**Type Erasure**: Generic type information removed at compile-time. Generics exist only for compile-time safety; runtime uses Object.

---

## QnA

**What's the difference between `<T>` and `<?>`?**

`<T>` is a type parameter bound to a specific type for the entire scope—once T is set, it remains that type. `<?>` is an unbounded wildcard representing any unknown type with no specific binding. T is used when you need consistency across operations; wildcard is used when you need flexibility without caring about the specific type.

**What is type erasure and why does it happen?**

Type erasure removes generic type information at compile-time for backward compatibility with older Java versions. Generic types exist only for compile-time safety; at runtime, generic types become Object. This allows new generic code to run on older JVMs. Developers see the benefits at compile-time while maintaining runtime compatibility.

**Can you create an instance of a generic type?**

No, `new T()` is not allowed because T is unknown at runtime due to type erasure. Workarounds include using factory methods, passing a Class object (`T obj = clazz.newInstance()`), or reflection. This is a limitation of Java's type erasure approach to generics.

**What's the difference between upper and lower bounded wildcards?**

Upper bounded wildcards (`? extends Type`) allow reading from a collection (covariance)—you know it's at most Type. Lower bounded wildcards (`? super Type`) allow writing to a collection (contravariance)—you know it's at least Type. PECS rule: Producer Extends, Consumer Super.

**Can generic types extend multiple types?**

Yes, `<T extends Class & Interface1 & Interface2>` allows multiple bounds. The class must be first; interfaces follow. This is useful when a type needs to implement multiple capabilities. For example, `<T extends Number & Comparable<T>>` requires T to be numeric and comparable.

**What's the diamond operator?**

The `<>` syntax allows compiler to infer type parameters. Instead of `List<String> list = new ArrayList<String>()`, you can write `List<String> list = new ArrayList<>()`. The compiler infers the right side type from the left side. Reduces verbosity while maintaining type safety.

---

## Code Examples

**Example - Generic Class:**
```java
public class Box<T> {
    private T content;
    
    public void add(T item) {
        this.content = item;
    }
    
    public T get() {
        return content;
    }
}

// Usage
Box<String> stringBox = new Box<>();
stringBox.add("Hello");
String value = stringBox.get();  // No casting needed

Box<Integer> intBox = new Box<>();
intBox.add(42);
```

**Example - Bounded Type Parameters:**
```java
public class NumberBox<T extends Number> {
    private T value;
    
    public void add(T val) {
        this.value = val;
    }
    
    public double getDouble() {
        return value.doubleValue();  // Can call Number methods
    }
}

// Valid: Integer, Double extend Number
NumberBox<Integer> iBox = new NumberBox<>();

// Invalid: String doesn't extend Number
// NumberBox<String> sBox = new NumberBox<>();  // Compile error
```

**Example - Wildcards:**
```java
// Upper bounded wildcard - can read
public void printNumbers(List<? extends Number> numbers) {
    for (Number n : numbers) {
        System.out.println(n);
    }
    // numbers.add(5);  // Cannot add (don't know exact type)
}

// Lower bounded wildcard - can write
public void addNumbers(List<? super Integer> numbers) {
    numbers.add(10);      // Can add Integer
    // Integer n = numbers.get(0);  // Cannot read safely
}
```

---

## Key Points

- **Raw Types Unsafe**: Using `List` without generics bypasses type checking—avoid in new code.
- **PECS Rule**: Producer Extends (reading), Consumer Super (writing) guides wildcard usage.
- **Unchecked Warnings**: `@SuppressWarnings("unchecked")` when necessary, but understand why.
- **Generic Arrays**: Cannot create arrays of generic types (`new List<String>[]`) due to type erasure.
- **Type Inference**: Compiler infers generic types; explicit specification sometimes needed for complex cases.
