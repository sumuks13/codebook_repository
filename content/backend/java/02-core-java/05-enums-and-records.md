---
{"publish":true,"title":"Enums & Records","cssclasses":""}
---

## Definitions

**Enum**: Type containing fixed set of constants. Can have constructors, methods, and fields. Automatically extend java.lang.Enum.

**Enum Constants**: Public static final instances. Accessed via `EnumName.CONSTANT`. Can be used in switch statements.

**Record**: Immutable data carrier class (Java 16+). Auto-generates constructor, getters, equals(), hashCode(), toString(). Reduces boilerplate.

**Sealed Class**: Class restricting which classes can extend it (Java 17+). Use permits keyword to list allowed subclasses.

---

## QnA

**Can you extend an Enum?**

No, all enums implicitly extend java.lang.Enum and cannot extend further or other classes. This prevents issues and maintains type safety. Enums are final by nature, though you can implement interfaces to add behavior.

**Can Enum implement interfaces?**

Yes, enums can implement interfaces and provide implementations. Each enum constant can have its own implementation through anonymous inner classes. This allows enums to define behavior beyond just constants.

**Can Enum have a constructor?**

Yes, enums can have private or package-private constructors (not public). Constructors are called automatically for each constant declaration with specified parameters. For example, `MONDAY(1, "Start")` calls constructor with those arguments.

**What's the difference between Enum and class?**

Enums define fixed instances; classes define blueprints for variable instances. Enum constants are singletons; you cannot instantiate new instances. Enums are automatically final. Enums are type-safe alternatives to string/int constants with compile-time checking.

**What are Records used for?**

Records are immutable data holders with automatic generation of getters, equals(), hashCode(), toString(), and canonical constructor. They reduce boilerplate for POJO (Plain Old Java Object) classes significantly. Use for simple data transfer objects, value objects, or data containers.

**Can Records extend classes or implement interfaces?**

Records cannot extend classes (implicitly extend Record). They can implement interfaces, allowing behavior addition while maintaining immutability. This design choice enforces immutability and simplicity.

---

## Code Examples

**Example - Enum with Constructor & Methods:**
```java
public enum Day {
    MONDAY(1, "Start of week"),
    TUESDAY(2, "Second day"),
    FRIDAY(5, "Fifth day"),
    SATURDAY(6, "Weekend"),
    SUNDAY(7, "Weekend");
    
    private final int dayNumber;
    private final String description;
    
    Day(int dayNumber, String description) {
        this.dayNumber = dayNumber;
        this.description = description;
    }
    
    public int getDayNumber() {
        return dayNumber;
    }
    
    public String getDescription() {
        return description;
    }
    
    public boolean isWeekend() {
        return this == SATURDAY || this == SUNDAY;
    }
}

// Usage
System.out.println(Day.MONDAY.getDayNumber());      // 1
System.out.println(Day.FRIDAY.isWeekend());         // false
System.out.println(Day.SATURDAY.getDescription());  // Weekend
```

**Example - Enum with switch:**
```java
Day day = Day.MONDAY;

switch (day) {
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY:
        System.out.println("Weekday");
        break;
    case SATURDAY, SUNDAY:
        System.out.println("Weekend");
        break;
}
```

**Example - Enum Values & Constants:**
```java
// Iterate all enum constants
for (Day d : Day.values()) {
    System.out.println(d);
}

// Get enum by name
Day d = Day.valueOf("MONDAY");

// Enum comparison
if (d == Day.MONDAY) {
    System.out.println("It's Monday");
}
```

**Example - Record (Java 16+):**
```java
// Before Records: verbose
public class PersonOld {
    private final String name;
    private final int age;
    
    public PersonOld(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String name() { return name; }
    public int age() { return age; }
    // + equals(), hashCode(), toString()
}

// After Records: concise
record Person(String name, int age) {
    // Compact constructor for validation
    public Person {
        if (age < 0) {
            throw new IllegalArgumentException("Age negative");
        }
    }
}

// Usage
Person p = new Person("Alice", 30);
System.out.println(p.name());  // Alice
System.out.println(p.age());   // 30
System.out.println(p);         // Person[name=Alice, age=30]
```

---

## Key Points

- **Enum Singleton**: Each enum constant is singleton; shared instance across JVM.
- **EnumSet/EnumMap**: Specialized collections for enums; highly efficient bit-set based.
- **Record Immutability**: Fields automatically final; no setters; safe to share.
- **Record Serialization**: Records properly serialize; no custom serialization needed typically.
- **Enum Safety**: Type-safe; compile-time checking; preferred over string/int constants.
