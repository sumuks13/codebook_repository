# Spring Data JPA

## Definitions

**Spring Data JPA**: Repository abstraction layer simplifying data access. Provides CRUD operations, query methods, pagination without manual implementation.

**Repository**: Interface extending JpaRepository/CrudRepository. Spring auto-implements CRUD methods and custom query methods.

**JpaRepository**: Interface extending CrudRepository adding batch operations, flush, deleteInBatch methods. Most commonly used repository type.

**Query Method**: Method in repository interface whose name/annotations define query logic. Spring generates SQL based on method signature.

**@Query**: Annotation specifying custom JPQL or native SQL query. Overrides default query generation from method name.

**Specification**: Dynamic query builder for complex queries. Type-safe alternative to query string concatenation.

**Page / Slice**: Result wrappers for paginated data. Page includes total count; Slice doesn't (faster for large datasets).

---

## Q&A

**Why use Spring Data JPA instead of plain Hibernate?**

Spring Data JPA provides repository abstraction; no custom implementation needed. Query method naming convention generates queries automatically. Pagination, sorting built-in. Less boilerplate; focus on business logic. Consistent interface across multiple data sources (JDBC, MongoDB via Spring Data).

**How do you define query methods in Spring Data JPA?**

Name methods following convention: `findBy`, `getBy`, `readBy`, `queryBy`, `searchBy`, `streamBy` followed by property name. Example: `findByEmail()` generates query filtering by email. Combine conditions: `findByEmailAndStatus()`. Add `@Query` for custom JPQL/SQL.

**What's the difference between @Query with JPQL and native SQL?**

JPQL operates on entities/properties; queries portable across databases. Native SQL uses database-specific syntax; faster but database-dependent. Use JPQL by default; native SQL when JPQL insufficient (stored procedures, complex database-specific queries).

**How do you implement pagination in Spring Data JPA?**

Define repository method accepting Pageable parameter: `Page<User> findByStatus(String status, Pageable pageable)`. Call with PageRequest: `userRepository.findByStatus("active", PageRequest.of(0, 10))`. Returns Page with data, total count, page info.

**What are specifications and when use them?**

Specifications enable dynamic query building; implement Specification interface defining Predicates. Use for complex filtering with multiple optional criteria. More type-safe than query string concatenation; cleaner than complex method names.

**Can you use named queries with Spring Data JPA?**

Yes, define `@NamedQuery` on entity, then use in repository with `@Query(name="...")`. Or use `@Query` directly in repository. Named queries cacheable, improved performance for frequently used queries.

---

## Code Examples

**Example - Repository Interface:**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    Optional<User> findByEmail(String email);
    
    List<User> findByStatusOrderByCreatedAtDesc(String status);
    
    @Query("from User u where u.email = :email")
    Optional<User> findUserByEmail(@Param("email") String email);
    
    @Query(value = "SELECT * FROM users WHERE age > ?1", 
           nativeQuery = true)
    List<User> findUsersOlderThan(int age);
}
```

**Example - Pagination:**
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public Page<User> getActiveUsers(int page, int size) {
        Pageable pageable = PageRequest.of(page, size, 
            Sort.by("createdAt").descending());
        return userRepository.findByStatus("active", pageable);
    }
}
```

**Example - Specification:**
```java
public class UserSpecification {
    public static Specification<User> hasEmail(String email) {
        return (root, query, cb) -> cb.equal(root.get("email"), email);
    }
    
    public static Specification<User> hasStatus(String status) {
        return (root, query, cb) -> cb.equal(root.get("status"), status);
    }
}

// Usage
List<User> users = userRepository.findAll(
    UserSpecification.hasEmail("test@example.com")
        .and(UserSpecification.hasStatus("active"))
);
```

---

## Key Points

- **Query Method Naming**: Master naming conventions for productivity; covers 80% of queries without custom @Query.
- **Pagination Performance**: For large datasets, use Slice instead of Page (doesn't count total).
- **Custom Repositories**: Extend repositories for complex logic; implement custom interface, inject repository.
- **Batch Operations**: Use batch methods for performance with large inserts/updates.
