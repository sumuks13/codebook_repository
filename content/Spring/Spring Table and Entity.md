tags : [[Spring Boot]]

The `@Entity` and `@Table` annotations are used to define and map Java classes to database tables. Here's the difference between the `@Entity` and `@Table` annotations:

1. `@Entity` Annotation:
   - The `@Entity` annotation is used to mark a Java class as a persistent entity, indicating that it should be mapped to a database table.
   - It is typically placed on the class level of a Java entity class.
   - The entity class represents a table in the database, and each instance of the class represents a row in that table.
   - By default, the entity name is derived from the class name, but it can be customized using the `name` attribute of the `@Entity` annotation.

   Example:
   ```java
   @Entity
   public class Student {
      // fields, getters, and setters
   }
   ```

2. `@Table` Annotation:
   - The `@Table` annotation is used to specify the details of the database table to which an entity class is mapped.
   - It is typically placed on the class level of a Java entity class, alongside the `@Entity` annotation.
   - The `@Table` annotation allows you to customize the table name, schema, and other properties associated with the table.

   Example:
   ```java
   @Entity
   @Table(name = "student")
   public class Student {
      // fields, getters, and setters
   }
   ```

By using both annotations together, you can define the entity class and specify the table details, such as the table name, schema, and any other properties associated with the table.

[source 8](https://www.javaguides.net/2020/10/defining-jpa-entity-entity-annotation.html)