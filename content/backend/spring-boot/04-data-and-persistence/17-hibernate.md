# Hibernate & ORM

## Definitions

**Hibernate**: Object-Relational Mapping (ORM) framework mapping Java objects to database tables. Reduces manual SQL, handles relationships, provides query language (HQL).

**Object-Relational Mapping (ORM)**: Technique mapping object-oriented model to relational database model. Objects → tables, properties → columns, relationships → foreign keys.

**Entity**: Java class representing database table. Marked with @Entity, each field maps to table column. Hibernate manages persistence, loading, caching.

**Mapping**: Configuration associating object properties with database columns. Done via annotations (@Column, @ManyToOne, @OneToMany) or XML.

**Session**: Hibernate session managing entity instances. Provides CRUD operations, query execution, transaction handling. Spring's SessionFactory abstraction simplifies session management.

**HQL (Hibernate Query Language)**: Object-oriented query language similar to SQL but operating on objects/properties instead of tables/columns. Platform-independent.

**JPA (Java Persistence API)**: Java standard for ORM. Hibernate is most popular JPA implementation. Spring Data JPA provides simplified repository abstraction.

---

## Q&A

**Why use ORM like Hibernate instead of raw SQL?**

ORM reduces boilerplate SQL, handles object mapping automatically, provides database independence (switch databases by changing dialect), manages relationships declaratively. Improved security (parameterized queries prevent SQL injection), easier refactoring (rename property, update mapping, done). Trade-off: ORM adds complexity, slightly slower than optimized SQL.

**What are entity relationships in Hibernate?**

Four types: @OneToOne (one entity relates to one another), @OneToMany (one relates to multiple), @ManyToOne (multiple relate to one), @ManyToMany (multiple relate to multiple). Defined via annotations with configuration (fetch type, cascade behavior, mappedBy for bidirectional). Relationships enable navigating from one entity to related entities.

**How do you configure column mapping in Hibernate?**

Use @Column annotation specifying database details: `@Column(name="user_name", length=100, nullable=false)`. Maps property to specific column, sets constraints. @Id marks primary key, @GeneratedValue specifies auto-generation strategy. @Transient excludes property from persistence.

**What's the difference between lazy and eager loading?**

Lazy loading defers loading related data until accessed (default for @OneToMany, @ManyToMany). Eager loading loads related data immediately (default for @ManyToOne, @OneToOne). Lazy reduces initial load time, eager avoids additional queries later. Choose based on usage patterns; lazy typically better for performance.

**How do you query entities using HQL?**

Use Session.createQuery() or Spring Data repositories. HQL example: `from User u where u.email = :email`. Parameter binding prevents SQL injection. HQL operates on objects, not tables: `from User` instead of `from users`. Named queries (@NamedQuery) cache frequently used queries.

**What's the N+1 query problem?**

Occurs when loading parent entity (1 query), then in loop loading each child (N queries). Solution: use JOIN FETCH in HQL, eager loading, or batch loading. Example: `from User u left join fetch u.orders` loads user and all orders in single query. Common performance issue in ORMs.

---

## Code Examples

**Example - Entity Mapping:**
```java
@Entity
@Table(name = "users")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "user_name", nullable = false, unique = true)
    private String username;
    
    @Column(nullable = false)
    private String email;
    
    @Column(length = 255)
    private String address;
    
    @Transient
    private String temporaryField;  // Not persisted
}
```

**Example - Entity Relationships:**
```java
@Entity
@Table(name = "users")
@Data
public class User {
    @Id
    @GeneratedValue
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders = new ArrayList<>();
}

@Entity
@Table(name = "orders")
@Data
public class Order {
    @Id
    @GeneratedValue
    private Long id;
    
    private String description;
    
    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "user_id")
    private User user;
}
```

**Example - HQL Queries:**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    @Query("from User u where u.email = :email")
    Optional<User> findByEmail(@Param("email") String email);
    
    @Query("from User u left join fetch u.orders where u.id = :id")
    Optional<User> findByIdWithOrders(@Param("id") Long id);
    
    @Query("from User u where u.username like :pattern")
    List<User> searchByUsername(@Param("pattern") String pattern);
}
```

**Example - Lazy vs Eager Loading:**
```java
// Lazy loading (default for collections)
@OneToMany(fetch = FetchType.LAZY)
private List<Order> orders;

// Eager loading
@ManyToOne(fetch = FetchType.EAGER)
private User user;

// Usage
User user = userRepository.findById(1L).get();
// Orders not loaded yet
List<Order> orders = user.getOrders();  // Additional query here
```

---

## Key Points

- **Cascading**: Use cascade = CascadeType.ALL carefully; can delete unintended entities. Typically use PERSIST, MERGE selectively.
- **Lazy Loading Exception**: Access lazy collection outside session/transaction causes LazyInitializationException. Load data within transaction or use eager loading.
- **Performance**: Monitor generated SQL with `hibernate.show_sql=true`. Use JOIN FETCH to avoid N+1 queries. Profile queries in production.
- **Bidirectional Relationships**: Define mappedBy on non-owning side. Owning side manages foreign key. Keep both sides in sync manually or use Lombok @Data carefully.
- **Identity Management**: Hibernate maintains identity: `session.load(User.class, 1) == session.load(User.class, 1)` returns true. Cache within session.
