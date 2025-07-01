tags : [[SQL]], [[PostgreSQL]]

Foreign key creation - Assuming this table:

```postgresql
CREATE TABLE students 
( 
  student_id SERIAL PRIMARY KEY,
  player_name TEXT
);
```

There are four different ways to define a foreign key (when dealing with a single column PK) and they all lead to the same foreign key constraint:

1. Inline without mentioning the target column:
    
    ```postgresql
    CREATE TABLE tests 
    ( 
       subject_id SERIAL,
       subject_name text,
       highestStudent_id integer REFERENCES students
    );
    ```
    
2. Inline with mentioning the target column:
    
    ```postgresql
    CREATE TABLE tests 
    ( 
       subject_id SERIAL,
       subject_name text,
       highestStudent_id integer REFERENCES students (student_id)
    );
    ```
    
3. Out of line inside the `create table`:
    
    ```postgresql
    CREATE TABLE tests 
    ( 
      subject_id SERIAL,
      subject_name text,
      highestStudent_id integer, 
      constraint fk_tests_students
         foreign key (highestStudent_id) 
         REFERENCES students (student_id)
    );
    ```
    
4. As a separate `alter table` statement:
    
    ```postgresql
    CREATE TABLE tests 
    ( 
      subject_id SERIAL,
      subject_name text,
      highestStudent_id integer
    );
    
    alter table tests 
        add constraint fk_tests_students
        foreign key (highestStudent_id) 
        REFERENCES students (student_id);
    ```
    