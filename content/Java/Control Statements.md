tags : [[Java/Conditional Statements]]

### **What are control statements in Java?**

Control statements are used to **change the flow of execution** in a program. They allow you to:
- **Make decisions** (conditional: `if`, `if-else`, `switch`)
- **Repeat actions** (loops: `for`, `while`, `do-while`)
- **Jump to other parts of code** (branching: `break`, `continue`, `return`)

### **What is a decision-making statement?**

It allows the program to **choose between different actions** based on conditions.

### **What is a loop control statement in Java?**

It lets you **repeat a block of code** multiple times.

### **When should you use a `for` loop?**

Use a `for` loop when the number of iterations is **known beforehand**.

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

### **How does a `while` loop work?**

A `while` loop executes the block **as long as the condition is true**.

```java
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}
```

### **What is the difference between `while` and `do-while` loop?**

| Feature         | `while`                | `do-while`            |
| --------------- | ---------------------- | --------------------- |
| Condition check | Before executing block | After executing block |
| Execution       | May not run even once  | Runs at least once    |

### **What are branching statements in Java?**

Branching statements allow you to **exit or skip parts of loops or methods**.

### **What does the `break` statement do?**

It **immediately exits** the current loop or `switch` block.

```java
for (int i = 0; i < 10; i++) {
    if (i == 5) break;
    System.out.println(i);
}
```

### **What is the use of `continue` statement?**

It skips the current iteration and **jumps to the next** loop cycle.

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue;
    System.out.println(i);
}
```

### **What is the purpose of the `return` statement?**

It **exits a method** and optionally returns a value.

```java
return x + y;
```

### **Can `break` be used outside a loop or switch?**

No. It causes a **compile-time error** if used outside a loop or `switch`.

### **What is a labeled break statement?**

It allows exiting **outer loops** using labels.

```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) break outer;
    }
}
```

### **What is a labeled continue statement?**

It skips to the next iteration of the **outer loop**.

```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) continue outer;
    }
}
```

### **What happens if a loop condition is always true?**

It results in an **infinite loop**, unless a `break` is used.

```java
while (true) {
    // infinite loop unless broken
}
```