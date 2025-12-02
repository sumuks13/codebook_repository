# Transaction Management

## Definitions

**Transaction**: Logical unit of work accessing database. ACID properties: Atomicity (all-or-nothing), Consistency (valid state), Isolation (concurrent independence), Durability (persistent).

**@Transactional**: Spring annotation enabling declarative transaction management. Wraps method with transaction, auto-commits on success, rolls back on exception.

**Propagation**: Behavior when calling transactional method from another transaction. Modes: REQUIRED (reuse or create), REQUIRES_NEW (always new), NESTED (savepoint), SUPPORTS (if exists).

**Isolation Level**: Concurrency control mechanism. Levels: READ_UNCOMMITTED (dirty reads), READ_COMMITTED (no dirty reads), REPEATABLE_READ (phantom reads possible), SERIALIZABLE (complete isolation).

**Rollback**: Undoing all changes when exception occurs. Automatic with @Transactional for unchecked exceptions; requires explicit handling for checked exceptions via `rollbackFor`.

**Save Point**: Marker within transaction allowing partial rollback. Used with NESTED propagation for sub-transactions.

---

## Q&A

**Why use @Transactional instead of manual transaction management?**

@Transactional simplifies code; Spring handles transaction demarcation, commit/rollback automatically. Declarative approach is cleaner than imperative try-catch-finally. Spring handles edge cases (nested transactions, exception handling) correctly. Enables transaction configuration via annotations, easier to change behavior.

**What's the default propagation in @Transactional?**

Default is PROPAGATION_REQUIRED: if transaction exists, reuse it; if not, create new one. Most common, suitable for most scenarios. PROPAGATION_REQUIRES_NEW always creates new transaction, suspending outer transaction (useful for independent operations like audit logging that shouldn't rollback parent).

**What's the difference between checked and unchecked exception handling in transactions?**

By default, @Transactional rolls back only on unchecked exceptions (RuntimeException). Checked exceptions (IOException, SQLException) don't trigger rollback. Override with `@Transactional(rollbackFor={SomeCheckedException.class})` to rollback on checked exceptions. Or use `noRollbackFor` to prevent rollback on specific exceptions.

**What are isolation levels and when do you change them?**

Isolation levels control concurrent transaction interference. READ_UNCOMMITTED fastest but allows dirty reads. READ_COMMITTED prevents dirty reads (default for most databases). REPEATABLE_READ prevents non-repeatable reads. SERIALIZABLE prevents all anomalies but slowest. Default READ_COMMITTED usually adequate; change for specific consistency requirements.

**Can you use @Transactional on class-level?**

Yes, class-level @Transactional applies to all public methods. Individual method annotations override class-level. Useful for consistent transaction settings across service class. However, method-level annotations better for explicit control; be cautious with class-level to avoid surprises.

**What happens if @Transactional method calls another @Transactional method?**

Depends on propagation. REQUIRED (default) reuses transaction; both methods in same transaction, single commit. REQUIRES_NEW creates new transaction; inner method in separate transaction, independent rollback. Nested creates savepoint; inner can rollback without affecting outer.

---

## Code Examples

**Example - Basic @Transactional:**
```java
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    
    @Transactional
    public void createOrder(Order order) {
        orderRepository.save(order);
        // If exception occurs here, transaction rolls back
    }
}
```

**Example - Rollback on Checked Exception:**
```java
@Service
public class PaymentService {
    
    @Transactional(rollbackFor = PaymentException.class)
    public void processPayment(Payment payment) throws PaymentException {
        // Checked exception will now trigger rollback
        validatePayment(payment);
        chargeCustomer(payment);
    }
}
```

**Example - Read-Only Transaction:**
```java
@Service
public class ReportService {
    
    @Transactional(readOnly = true)
    public List<Report> getReports() {
        // Optimized for read-only; prevents accidental writes
        return reportRepository.findAll();
    }
}
```

**Example - Nested Transactions with NESTED Propagation:**
```java
@Service
public class UserService {
    
    @Transactional
    public void createUserWithAudit(User user) {
        userRepository.save(user);
        auditLog(user);  // Independent transaction
    }
    
    @Transactional(propagation = Propagation.NESTED)
    private void auditLog(User user) {
        // Savepoint created; can rollback independently
        auditRepository.save(new Audit(user));
    }
}
```

**Example - Transaction Timeout:**
```java
@Service
public class LongRunningService {
    
    @Transactional(timeout = 30)  // 30 seconds
    public void processLargeDataset() {
        // If method takes > 30 seconds, transaction rolls back
        for (int i = 0; i < largeList.size(); i++) {
            process(largeList.get(i));
        }
    }
}
```

---

## Key Points

- **Declarative vs Programmatic**: Declarative (@Transactional) preferred; easier, cleaner code. Programmatic (TransactionTemplate) used when complex logic needed.
- **Service Layer Transactions**: Place @Transactional on Service methods, not Repository or Controller. Service layer coordinates multiple operations atomically.
- **No Rollback on Checked Exceptions**: Remember default only rollbacks RuntimeException; configure rollbackFor for business exceptions if needed.
- **Read-Only Optimization**: Use readOnly=true for queries; database optimizes, prevents accidental writes.
- **Transaction Scope**: Keep transactions small, focused on single business operation. Large transactions lock resources longer, reduce concurrency.
