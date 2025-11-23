---
{"publish":true,"title":"Strings","cssclasses":""}
---

## Definitions

**String**: Immutable sequence of characters. Objects in Java, not primitive types. Internally backed by char arrays.

**String Immutability**: Once created, String objects cannot be modified. Any modification creates a new String object.

**String Pool**: Memory optimization where identical string literals share same memory location. Interning mechanism.

**String Comparison**: `==` compares references; `.equals()` compares content; `.equalsIgnoreCase()` case-insensitive comparison.

**StringBuilder/StringBuffer**: Mutable alternatives to String for efficient string concatenation. StringBuffer is synchronized; StringBuilder is not.

---

## QnA

**What's the difference between String and StringBuilder?**

String is immutable—once created, it cannot be changed, and any modification creates a new String object. StringBuilder is mutable and allows in-place modifications. Use String for fixed values and security; use StringBuilder for frequent concatenation to avoid memory overhead of creating many String objects. StringBuilder is more efficient in loops with string concatenation.

**What's the difference between `==` and `.equals()` for Strings?**

The `==` operator compares object references (whether they point to the same object in memory), while `.equals()` compares the actual content of the strings. Two different String objects with identical content return false for `==` but true for `.equals()`. Always use `.equals()` when comparing string content; use `==` only when checking if variables reference the same object.

**Why is String immutable?**

String immutability provides multiple benefits: thread-safety (no synchronization needed across threads), enables string pooling (identical literals share memory), supports caching of hash code for fast HashMap lookups, and provides security (prevents malicious code from changing sensitive strings like passwords after creation).

**How do you convert a String to an array?**

Use `.toCharArray()` to convert to a char array, or `.split("delimiter")` to convert to a String array. For example, `"Hello".toCharArray()` returns `['H', 'e', 'l', 'l', 'o']`, and `"a,b,c".split(",")` returns `["a", "b", "c"]`. Split is useful for parsing delimited data.

**What happens when you concatenate Strings in a loop?**

Each concatenation creates a new String object, resulting in O(n²) complexity for n concatenations. In a loop, this creates unnecessary garbage. Use StringBuilder instead: `StringBuilder sb = new StringBuilder(); for(i) { sb.append(...); }` provides O(n) complexity by modifying a mutable buffer rather than creating new objects.

**What's the difference between StringBuffer and StringBuilder?**

StringBuffer is synchronized and thread-safe, making it suitable for multi-threaded environments but slower due to synchronization overhead. StringBuilder is not synchronized and faster, suitable for single-threaded code. Since Java 5, StringBuilder is preferred in single-threaded code; use StringBuffer only when sharing a string builder across threads.

---

## Code Examples

**Example - String Immutability:**
```java
String str = "Hello";
String modified = str.toUpperCase();
System.out.println(str);      // Hello (unchanged)
System.out.println(modified); // HELLO (new object)

// Every modification creates new string
String original = "Java";
String result = original.replace("a", "o");
System.out.println(original); // Java (not Jova)
```

**Example - String Comparison:**
```java
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");

System.out.println(s1 == s2);           // true (same reference from pool)
System.out.println(s1 == s3);           // false (different objects)
System.out.println(s1.equals(s3));      // true (same content)
System.out.println(s1.equalsIgnoreCase("hello")); // true
```

**Example - String Methods:**
```java
String str = "Hello Java World";

// Length and character access
System.out.println(str.length());       // 16
System.out.println(str.charAt(0));      // H

// Substring
System.out.println(str.substring(0, 5)); // Hello
System.out.println(str.substring(6));    // Java World

// Case conversion
System.out.println(str.toUpperCase());  // HELLO JAVA WORLD
System.out.println(str.toLowerCase());  // hello java world

// Searching
System.out.println(str.indexOf("Java"));     // 6
System.out.println(str.contains("World"));   // true
System.out.println(str.startsWith("Hello")); // true
System.out.println(str.endsWith("World"));   // true

// Splitting and joining
String[] words = str.split(" ");
for (String word : words) {
    System.out.println(word);  // Hello, Java, World
}

String joined = String.join("-", words); // Hello-Java-World
```

**Example - StringBuilder for Concatenation:**
```java
// Bad: O(n²) complexity due to creating new String each iteration
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i + ",";  // Creates new String each time
}

// Good: O(n) complexity using StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i).append(",");  // Modifies internal buffer
}
String result2 = sb.toString();

// Method chaining with StringBuilder
StringBuilder str = new StringBuilder();
str.append("Hello")
   .append(" ")
   .append("World")
   .insert(0, ">> ")
   .reverse();
System.out.println(str.toString()); // dlroW ajaV >> olleH
```

**Example - String Pool Behavior:**
```java
// String literals pool
String a = "Java";
String b = "Java";
System.out.println(a == b);  // true (same pool reference)

// new String always creates new object
String c = new String("Java");
System.out.println(a == c);  // false (different objects)
System.out.println(a.equals(c)); // true (same content)

// intern() adds to pool
String d = new String("Java").intern();
System.out.println(a == d);  // true (added to pool)
```

---

## Key Points

- **String Immutability**: Essential for HashMap keys, thread-safety, and security in libraries. Never rely on string mutability.
- **String Pool**: JVM caches string literals to save memory. Identical literals share same object. `intern()` adds custom strings to pool.
- **String Concatenation**: `+` operator creates multiple intermediate String objects. Use StringBuilder in loops for efficiency.
- **Performance**: StringBuffer slower due to synchronization; StringBuilder preferred for single-threaded contexts; both better than concatenation.
- **Null Handling**: `.equals()` throws NullPointerException if string is null; use `Objects.equals(s1, s2)` for null-safe comparison.
