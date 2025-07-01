tags : [[Algorithms]]
links : https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898

1. **Single Responsibility Principle (SRP)**:

Imagine a Restaurant: A waiter shouldn't be responsible for cooking, cleaning tables, and managing the cash register. Each role (class) should have a clear and distinct responsibility. In programming, a class like Order should focus on managing order items, not sending confirmations (handled by a separate EmailService class).

2. **Open/Closed Principle (OCP)**:

Think of a Toolbox: You should be able to add new tools (features) without modifying the existing toolbox itself (changing existing code). This principle encourages designing classes in a way that lets you extend functionalities through inheritance or interfaces, like adding new power tools without redesigning the entire toolbox.

3. **Liskov Substitution Principle (LSP)**:

Imagine Toy Blocks: All toy blocks (subtypes) should work seamlessly with regular blocks (base type). If you replace a rectangular block with a square block, things shouldn't fall apart. This principle ensures that subclasses behave consistently with their parent classes, like a square block fitting with other blocks even though it has a specific shape.

4. **Interface Segregation Principle (ISP)**:

Think of a Gym Membership: You shouldn't have to pay for a full membership with pool access if you only want to use the weight room. This principle promotes creating smaller, more specific interfaces that cater to different client needs. In programming, a PaymentProcessor interface could be separated into specific interfaces like CreditCardProcessor and CashOnDeliveryProcessor.

5. **Dependency Inversion Principle (DIP)**:

Imagine a Light Switch: The switch (high-level module) shouldn't depend on the specific type of light bulb (low-level module) it controls. This principle encourages relying on abstractions (interfaces) instead of concrete implementations, allowing flexibility in choosing the actual light bulb (implementation) without affecting the switch functionality.