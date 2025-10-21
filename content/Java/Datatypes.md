tags : [[Java/Java]]

### **What are data types in Java?**

Data types specify the **type of data** a variable can hold.  
Java is a **strongly typed language**, so each variable must have a declared type.

### **What are the main categories of data types in Java?**

1. **Primitive data types** (8 types)
2. **Non-primitive (Reference) data types** like arrays, classes, strings, etc.

### **What are the 8 primitive data types in Java?**

| Type      | Size    | Example                            | Default Value |
| --------- | ------- | ---------------------------------- | ------------- |
| `byte`    | 1 byte  | -128 to 127                        | 0             |
| `short`   | 2 bytes | -32,768 to 32,767                  | 0             |
| `int`     | 4 bytes | Commonly used for integers         | 0             |
| `long`    | 8 bytes | Larger integer values              | 0L            |
| `float`   | 4 bytes | Decimal values with less precision | 0.0f          |
| `double`  | 8 bytes | Decimal values with more precision | 0.0d          |
| `char`    | 2 bytes | Single character (`'A'`)           | `\u0000`      |
| `boolean` | 1 bit   | `true` or `false`                  | false         |

### **Why is `String` not a primitive data type in Java?**

Because it is a **class** in Java. It holds **a sequence of characters**, and is part of the **reference data types**.

### **What is type casting in Java?**

Type casting is converting one data type into another. There are two types:
- **Implicit** (smaller to larger): `int x = 10; double y = x;`
- **Explicit** (larger to smaller): `double d = 10.5; int x = (int) d;`

### **What happens during overflow and underflow in primitive types?**

- **Underflow**: when it goes below the min limit.
- **Overflow**: when a value exceeds the max limit, it wraps around.
   
```java
byte b = 127;
b++;  // becomes -128
```

### **Is `char` a numeric type in Java?**

Yes. `char` holds a **Unicode number**, so it can be treated like an integer.

```java
char c = 'A';       // 65
int code = c + 1;   // 66
```

### **What is the range of an `int` in Java?**

- **-2,147,483,648 to 2,147,483,647**
- 32-bit signed integer (uses 2's complement)

### **What is the size of a `boolean` in Java?**

Although it logically holds only `true` or `false`, its exact memory size is **not precisely defined** by the Java specification. JVM handles it internally, typically as 1 byte. 8 bit = 1 byte.