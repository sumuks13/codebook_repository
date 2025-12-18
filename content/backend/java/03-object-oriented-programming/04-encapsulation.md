---
{"publish":true,"title":"Encapsulation","cssclasses":""}
---

## Definitions

**Encapsulation**: Bundling data (variables) and methods into a single unit (class) and hiding internal details. Controlled access via getters/setters.

**Data Hiding**: Private members exposed through controlled public methods. Prevents invalid state changes and unauthorized access.

**Getter Method**: Public method returning private variable value. Allows read access with validation/control logic.

**Setter Method**: Public method setting private variable value. Enables validation before modification.

**Access Control**: Using access modifiers (private, package-private, protected, public) to control visibility and prevent misuse.

---

## QnA

**Why use getters and setters instead of public fields?**

Getters and setters provide control: validation of data before modification, lazy loading of expensive resources, notification to other objects when state changes, and future flexibility to change implementation without breaking API. For example, a setter can validate age is positive before assignment, whereas public field allows any value assignment.

**Can a getter change the object's state?**

Technically yes, but it's bad practice violating the principle of least surprise. Getters should be side-effect free query operations; setters should change state. If a getter modifies state, code becomes unpredictable and harder to debug. Follow conventions: getters query, setters mutate.

**What's the benefit of encapsulation?**

Encapsulation hides implementation details, enables validation and invariant maintenance, allows internal refactoring without affecting external code, and supports the single responsibility principle. Changes to internal data structures don't break code depending on public interface.

**Should sensitive data be exposed in getters?**

No, return copies or immutable views instead. For example, return `new ArrayList(internalList)` instead of the list reference. This prevents external code from modifying internal state. Sensitive data like passwords should never be directly accessible or logged.

**Is making all fields private always required?**

No, but it's recommended. Public constants (`static final`) are acceptable for true immutable values. Public mutable fields violate encapsulation. Be pragmatic: private fields with getters/setters for state that could change; public static final for true constants.

**What's an "immutable object"?**

An immutable object's state cannot change after creation. Achieved via private final fields, private constructor, defensive copying of mutable parameters. Examples: String, Integer. Immutable objects are thread-safe and can be safely shared. Use immutability when consistency matters.

---

## Code Examples

**Example - Basic Encapsulation:**
```java
public class BankAccount {
    private String accountNumber;
    private double balance;
    private String accountHolder;
    
    public BankAccount(String accountNumber, String holder) {
        this.accountNumber = accountNumber;
        this.accountHolder = holder;
        this.balance = 0;
    }
    
    public double getBalance() {
        return balance;
    }
    
    public String getAccountInfo() {
        return "Account: ***" + accountNumber.substring(accountNumber.length() - 4);
    }
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        } else {
            throw new IllegalArgumentException("Deposit amount must be positive");
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        } else {
            throw new IllegalArgumentException("Invalid withdrawal amount");
        }
    }
}
```

**Example - Defensive Copying:**
```java
import java.util.ArrayList;
import java.util.List;

public class StudentGrades {
    private String name;
    private List<Integer> grades;
    
    public StudentGrades(String name) {
        this.name = name;
        this.grades = new ArrayList<>();
    }
    
    // Bad: Returns reference to internal list
    public List<Integer> getGradesBad() {
        return grades;  // Caller can modify!
    }
    
    // Good: Returns defensive copy
    public List<Integer> getGrades() {
        return new ArrayList<>(grades);
    }
    
    public void addGrade(int grade) {
        if (grade >= 0 && grade <= 100) {
            grades.add(grade);
        }
    }
}
```

---

## Key Points

- **Single Responsibility**: Each class encapsulates one concept; methods related to that concept.
- **Information Hiding**: Hide details; expose stable interface. Allows internal refactoring without breaking clients.
- **Invariants**: Maintain object state consistency through controlled access. Setters enforce business rules.
- **Defensive Copying**: Return copies for mutable objects; prevents external modification of internal state.
- **Immutability Pattern**: Combine final fields, private constructor, defensive copying for thread-safe immutable objects.
