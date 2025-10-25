tags: [[Java/Control Statements]]
### **What are conditional statements in Java?**

- Conditional statements allow Java programs to **make decisions** based on **boolean expressions**. 
- They control the **flow of execution** depending on whether a condition is true or false.

### **What are the main types of conditional statements in Java?**

- `if` statement
- `if-else` statement
- `if-else if` ladder
- `switch` statement
- Ternary operator (`?:`)

### **How does an `if` statement work in Java?**

An `if` statement checks a condition and **executes the block only if the condition is true**.

```java
if (x > 0) {
    System.out.println("Positive number");
}
```

### **What is the syntax of the `if-else` statement?**

The `if-else` allows choosing **between two blocks**.

```java
if (x % 2 == 0) {
    System.out.println("Even");
} else {
    System.out.println("Odd");
}
```

### **What is an `if-else-if` ladder?**

Used when there are **multiple conditions** to check.

```java
if (score >= 90) {
    System.out.println("Grade A");
} else if (score >= 75) {
    System.out.println("Grade B");
} else {
    System.out.println("Grade C");
}
```

### **What is the `switch` statement used for?**

The `switch` statement allows selecting one block to run from **multiple cases**, based on the value of an expression.

```java
int day = 2;
switch (day) {
    case 1: System.out.println("Monday"); break;
    case 2: System.out.println("Tuesday"); break;
    default: System.out.println("Invalid day");
}
```

---

### **Which data types can be used in a `switch` statement in Java?**

- `byte`, `short`, `int`, `char`
- `String` (Java 7 onwards)
- `enum`
- Wrapper types like `Integer`, `Character`

Note: boolean conditions cannot be used in a switch statement.

### **What happens if you forget `break` in a `switch` statement?**

Without `break`, <mark style="background: #FFF3A3A6;">fall-through</mark> occurs — execution continues into the next case.

```java
int x = 1;
switch (x) {
    case 1: System.out.println("One");
    case 2: System.out.println("Two"); // This will also run
}
```

### **What is the ternary operator in Java?**

It is a **compact version of if-else**. It returns one of two values based on the condition.

```java
int max = (a > b) ? a : b;
```

### **What are nested `if` statements?**

An `if` inside another `if`. Used for **multi-level decisions**.

```java
if (x > 0) {
    if (x % 2 == 0) {
        System.out.println("Positive Even");
    }
}
```

### **What are the rules for writing conditional expressions?**

- The condition inside `if` must be **boolean**
- Use `{}` braces to avoid bugs even for single-line blocks
- Avoid deep nesting for better readability

### **Can a switch statement have duplicate case values?**

No. **Duplicate case values will cause a compile-time error**.

### **Can `default` come before `case` in a switch block?**

Yes. The position of `default` does not matter, but it’s conventionally written at the end.

### **What is short-circuiting in Java conditions?**

In conditions using `&&` or `||`:
- `A && B`: if A is false, B is **not evaluated**
- `A || B`: if A is true, B is **not evaluated**

This improves performance and avoids unnecessary operations.