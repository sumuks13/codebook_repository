---
{"publish":true,"title":"Java Fundamentals","cssclasses":""}
---

## Definitions

**Conditional Statements**: Control flow structures that execute different code based on boolean conditions. Include if-else, ternary operator, and switch statements.

**Loops**: Repetition structures that execute code blocks multiple times. Java supports for, while, do-while, and enhanced for-loop (for-each).

**Loop Control**: Keywords like break (exits loop) and continue (skips current iteration) to alter loop behavior.

---

## QnA

**What is the difference between while and do-while loops?**

A while loop checks the condition before executing the loop body, so it may not run at all if the condition is false initially. A do-while loop executes the loop body first and then checks the condition, which means it runs at least once regardless of the condition. Use while loops when you want to validate before executing; use do-while when you need at least one execution.

**When would you use an enhanced for-loop vs a regular for loop?**

Enhanced for-loops (for-each) are cleaner for iterating through all elements of a collection or array when you don't need the index. Regular for loops give you explicit control over the iteration counter and are necessary when you need the index value, need to skip elements, or need to iterate backwards. Use for-each for simple iteration; use regular for when you need index-based logic.

**Can you use break outside of a loop or switch statement?**

No, the break keyword is only valid inside loops (for, while, do-while) or switch statements. Using it elsewhere causes a compile error. Break immediately exits the nearest enclosing loop or switch, transferring control to the statement after the loop/switch.

**What's the difference between break and continue?**

Break exits the loop entirely, jumping to the first statement after the loop. Continue skips the current iteration and jumps to the next iteration of the loop. Break is useful when you want to terminate based on a condition; continue is useful for skipping certain iterations while continuing the loop.

**Can if-else be replaced with a ternary operator?**

Yes, simple if-else statements can be replaced with the ternary conditional operator. Syntax: `result = (condition) ? trueValue : falseValue;`. However, ternary operators should only replace simple if-else because nested ternaries become unreadable. Keep ternary operators concise; use if-else for complex logic.

**What happens with switch statement fallthrough?**

Without a break statement, execution continues from one case to the next case (fallthrough). For example, if case 1 executes and has no break, the code in case 2 also executes. This is sometimes intentional when multiple cases should execute the same code, but it's often a bug. Always use break to prevent unintended fallthrough.

---

## Code Examples

**Example - Conditionals:**
```java
int score = 85;

// if-else
if (score >= 90) {
    System.out.println("Grade A");
} else if (score >= 80) {
    System.out.println("Grade B");
} else {
    System.out.println("Grade C");
}

// Ternary operator
String grade = (score >= 80) ? "Pass" : "Fail";

// Switch statement
switch (score / 10) {
    case 9:
    case 10:
        System.out.println("Excellent");
        break;
    default:
        System.out.println("Average");
}
```

**Example - Loops:**
```java
// For loop
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// Enhanced for-loop
for (String item : list) {
    System.out.println(item);
}

// While loop
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

// Do-while loop
int j = 0;
do {
    System.out.println(j);
    j++;
} while (j < 5);  // Condition checked after execution
```

---

## Key Points

- **Switch Fallthrough**: Without break, execution continues to next case. Use break or design intentional fallthrough carefully.
- **Loop Labels**: Can label loops to control nested loop breaks: `outerLoop: for(...)` then `break outerLoop;`
- **Performance**: Enhanced for-loop internally uses Iterator; regular for-loop is faster for indexed access on arrays.
- **Scope**: Variables declared in loop initialization (for loop) are scoped to that loop only.
- **do-while Guarantee**: do-while guarantees at least one execution; while may not execute at all.
