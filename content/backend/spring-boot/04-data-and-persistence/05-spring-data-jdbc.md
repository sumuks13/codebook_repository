---
{"publish":true,"title":"Spring Data JDBC","cssclasses":""}
---

## Definitions

**Spring Data JDBC**: Lightweight alternative to JPA focusing on simpler domain models. Less magic, more control over SQL. No lazy loading, no dirty checking, no sessions.

**Aggregate**: Domain model concept where entity and related objects form single unit. Root entity manages lifecycle of entire aggregate. Changes to aggregate persisted atomically.

**Repository**: Interface extending CrudRepository. Provides CRUD operations without JPA complexity. Custom queries via `@Query` annotation.

**Immutability**: Spring Data JDBC encourages immutable entities. Updates create new objects rather than modifying existing. Reduces bugs from shared mutable state.

**Explicit Loading**: No lazy loading; all related objects loaded eagerly. Prevents N+1 query problems but requires careful modeling to avoid over-fetching.

**No Persistence Context**: Unlike JPA, no first-level cache or change tracking. Every operation hits database immediately. Simpler but less automatic.

---

## Q&A

**When should you use Spring Data JDBC instead of JPA?**

Use Spring Data JDBC for simpler domain models, microservices preferring explicit control, systems avoiding ORM complexity. JPA better for complex entity graphs, bidirectional relationships, lazy loading requirements. JDBC more predictable, less "magic"; JPA more feature-rich, automatic.

**How does Spring Data JDBC differ from JPA?**

Spring Data JDBC: no session/persistence context, no lazy loading, no dirty checking, explicit saves required. JPA: managed entities, automatic change tracking, lazy loading, complex caching. JDBC simpler, more predictable; JPA more powerful, more complex.

**What is the aggregate pattern in Spring Data JDBC?**

Aggregate groups related entities into single transactional unit with root entity. Root manages lifecycle; deleting root deletes entire aggregate. Example: Order (root) with OrderItems (children). Spring Data JDBC enforces aggregate boundaries; no standalone access to children.

**How do you handle relationships in Spring Data JDBC?**

Use `@MappedCollection` for one-to-many relationships within aggregates. Foreign keys managed manually. No `@OneToMany` or `@ManyToOne` like JPA. Relationships simpler but require explicit modeling. Bidirectional relationships not supported; unidirectional only.

**Can you use custom queries in Spring Data JDBC?**

Yes, use `@Query` annotation with SQL (not JPQL). Repository methods execute custom SQL. Example: `@Query("SELECT * FROM users WHERE email = :email")`. Native SQL only; no object query language abstraction.

**How does Spring Data JDBC handle transactions?**

Works with Spring's `@Transactional` like JPA. Repository operations participate in transactions. No automatic dirty checking; explicit `save()` required. Simpler transaction semantics; changes not automatically tracked.

---

## Code Examples

**Example - Entity with Aggregate:**
```java
@Table("orders")
public class Order {
    @Id
    private Long id;
    
    private LocalDateTime orderDate;
    private String customerName;
    
    @MappedCollection(idColumn = "order_id")
    private Set<OrderItem> items = new HashSet<>();
    
    // Immutable style - use constructor
    public Order(String customerName) {
        this.orderDate = LocalDateTime.now();
        this.customerName = customerName;
    }
}

@Table("order_items")
public class OrderItem {
    @Id
    private Long id;
    
    private String productName;
    private int quantity;
    private BigDecimal price;
}
```

**Example - Repository Interface:**
```java
@Repository
public interface OrderRepository extends CrudRepository<Order, Long> {
    
    @Query("SELECT * FROM orders WHERE customer_name = :name")
    List<Order> findByCustomerName(@Param("name") String name);
    
    @Query("SELECT * FROM orders WHERE order_date >= :startDate")
    List<Order> findOrdersAfter(@Param("startDate") LocalDateTime startDate);
}
```

**Example - Service Layer:**
```java
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    
    @Transactional
    public Order createOrder(String customerName, List<OrderItem> items) {
        Order order = new Order(customerName);
        order.getItems().addAll(items);
        return orderRepository.save(order);
    }
    
    public List<Order> getCustomerOrders(String customerName) {
        return orderRepository.findByCustomerName(customerName);
    }
}
```

**Example - Configuration:**
```java
@Configuration
@EnableJdbcRepositories(basePackages = "com.example.repository")
public class JdbcConfig {
    
    @Bean
    public DataSource dataSource() {
        return DataSourceBuilder.create()
            .url("jdbc:mysql://localhost:3306/db")
            .username("user")
            .password("password")
            .build();
    }
}
```

---

## Key Points

- **Simplicity**: Spring Data JDBC simpler than JPA; fewer abstractions, easier debugging, more predictable queries.
- **Performance**: No lazy loading or session overhead; queries execute immediately. Good for read-heavy workloads.
- **Explicit Updates**: Must call `save()` explicitly; no automatic dirty checking. More verbose but clearer.
- **Aggregate Boundaries**: Design aggregates carefully; entire aggregate loaded/saved together. Keep aggregates small for performance.
- **No Bidirectional**: One-way relationships only; parent references children, not vice versa. Simpler model, less complexity.
