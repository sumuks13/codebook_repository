# JPA Entity Lifecycle

## Definitions

**Entity Lifecycle**: Sequence of states entity goes through: Transient, Managed, Detached, Removed. Each state determines persistence behavior.

**Transient**: Entity not associated with persistence context. No database identity. Created with `new` keyword, not persisted.

**Managed**: Entity associated with active persistence context. Changes automatically tracked, persisted on transaction commit. Returned from repository.find*() methods.

**Detached**: Entity previously managed but context closed. Changes not tracked. Can be reattached via merge().

**Removed**: Entity marked for deletion. Persisted as deletion on commit.

**Persistence Context**: Cache of managed entities for current transaction. Ensures single instance per entity ID, tracks changes.

**@PrePersist / @PostPersist**: Lifecycle callbacks executed before/after entity insertion. Used for initialization, auditing.

**@PreUpdate / @PostUpdate**: Callbacks before/after updates.

---

## Q&A

**What are the entity lifecycle states?**

Transient (new, unassociated with database), Managed (associated with persistence context, changes tracked), Detached (previously managed, context closed), Removed (marked for deletion). Understanding states crucial for knowing when data persists, when changes tracked. Transitions occur via entityManager operations.

**When does an entity transition from transient to managed?**

Transition occurs when entity saved: `entityManager.persist(entity)` or `repository.save(entity)`. Entity gets database ID, managed by persistence context. Changes tracked automatically within transaction.

**How do you reattach a detached entity?**

Use `entityManager.merge(detachedEntity)` or `repository.save(detachedEntity)`. Returns managed instance; updates not on detached object. Alternative: keep persistence context open or use refresh().

**What's the difference between persist() and merge()?**

persist() inserts new entity, returns void. merge() handles both insert (if new) and update (if detached), returns managed instance. Use persist() for new entities, merge() for reattaching. merge() safer for mixed scenarios.

**What are lifecycle callbacks and when use them?**

Callbacks execute at entity lifecycle events: @PrePersist (before insert), @PostPersist (after insert), @PreUpdate (before update), @PreRemove (before delete). Use for automatic auditing (set timestamps, user info), validation, or cleanup. Define as methods annotated with callbacks.

**What happens if you modify a managed entity?**

Changes automatically tracked within transaction; persisted on commit without explicit save(). This is powerful but can cause surprises if not understood. Be careful; unexpected changes persist.

---

## Code Examples

**Example - Entity Lifecycle:**
```java
@Entity
@Data
public class User {
    @Id
    @GeneratedValue
    private Long id;
    
    private String name;
    
    @CreationTimestamp
    @Column(nullable = false, updatable = false)
    private LocalDateTime createdAt;
    
    @UpdateTimestamp
    private LocalDateTime updatedAt;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }
    
    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```

**Example - Transient to Managed:**
```java
// Transient - new, not in context
User user = new User("Alice");
System.out.println(user.getId());  // null

// Managed - after save
User managed = repository.save(user);
System.out.println(managed.getId());  // database ID assigned
```

**Example - Detached to Managed:**
```java
User user = repository.findById(1L).get();  // Managed
// Context closed or flush occurred
// User now detached

user.setName("Updated");  // Change not tracked

User reattached = repository.save(user);  // Reattached, merged
System.out.println(reattached.getName());  // "Updated"
```

---

## Key Points

- **Persistence Context**: Session/transaction scope; same entity ID returns same instance (first-level cache).
- **Lazy Exceptions**: Access lazy collection outside transaction causes LazyInitializationException; load data within transaction.
- **Dirty Checking**: Managed entities automatically flushed on transaction commit; no explicit save needed for updates.
- **Cascading**: Configure cascade types carefully; cascading deletes can unintentionally remove related entities.
