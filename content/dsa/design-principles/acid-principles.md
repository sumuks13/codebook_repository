---
{"publish":true,"title":"ACID Principles","cssclasses":""}
---

tags: [database](database/)

The **ACID properties** are a set of principles used to guarantee that database transactions are processed reliably.

**Atomicity (A)**: Atomicity guarantees that a transaction is treated as a single, indivisible unit of work. This means the entire transaction either **succeeds completely** or **fails completely**.

- If all statements within the transaction execute successfully, the transaction is **committed**.
- If any single part of the transaction fails (due to a power outage, error, or system crash), the entire transaction is **rolled back** to its original state, and no changes are made to the database.  

**Analogy:** Transferring money between bank accounts. The operation involves two steps: debiting account A and crediting account B. Atomicity ensures that if the debit succeeds but the credit fails, the debit is undone, leaving the money in account A.

---

**Consistency (C)**: Consistency ensures that a transaction only moves the database from one **valid state** to another.

- Any data written to the database must be valid according to all defined rules, constraints, triggers, and cascades.
- If a transaction violates any of these rules, the entire transaction is rolled back, preventing the database from entering an invalid state.

**Analogy:** If a column is defined as `NOT NULL`, a transaction attempting to insert a `NULL` value will be rolled back to maintain consistency.

---

**Isolation (I)**: Isolation guarantees that **concurrently executing transactions** are processed in a way that makes it seem as if each transaction is running _alone_ or _sequentially_.

- The intermediate state of one transaction is not visible to other transactions running at the same time.
- This property prevents concurrency problems like dirty reads, non-repeatable reads, and phantom reads.

**Analogy:** Isolation is like having multiple people use separate **ATMs** simultaneously to access the same bank account. The system ensures that each withdrawal or deposit processes fully without interference, making it appear as if only one person is accessing the funds at a time, thus preventing one transaction from seeing the intermediate, inconsistent state of another.

**Note:** Database systems achieve isolation through various **isolation levels** (e.g., Read Uncommitted, Read Committed, Repeatable Read, Serializable), which offer different trade-offs between concurrency and data integrity.

---

**Durability (D)**: Durability ensures that once a transaction has been **committed**, the changes are **permanent** and will survive any subsequent system failures, such as power loss, crashes or reboots.

- The committed data is typically written to **non-volatile storage** (like a hard disk or solid-state drive) and recorded in transaction logs.

**Analogy:** When a bank confirms your deposit, the transaction is durable. Even if the system immediately crashes afterward, the deposit record remains intact and cannot be lost.