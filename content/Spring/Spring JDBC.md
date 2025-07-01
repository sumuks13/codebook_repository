
Spring **JdbcTemplate** is a powerful mechanism to connect to the database and execute SQL queries. It internally uses JDBC api, but eliminates a lot of problems of JDBC API.

## Problems of JDBC API

- We need to write a lot of code before and after executing the query, such as creating connection, statement, closing resultset, connection etc.
- We need to perform exception handling code on the database logic.
- We need to handle transaction.
- Repetition of all these codes from one to another database logic is a time consuming task.

## Spring Jdbc Approaches

- JdbcTemplate
- NamedParameterJdbcTemplate
- SimpleJdbcTemplate
- SimpleJdbcInsert and SimpleJdbcCall

### JDBC Template:

It is the central class in the Spring JDBC support classes. It takes care of creation and release of resources such as creating and closing of connection object etc. We can perform all the database operations by the help of JdbcTemplate class such as insertion, updation, deletion and retrieval of the data from the database.

```sql
--table structure.
create table employee(  
  id number(10),  
  name varchar2(100),  
  salary number(10)  
)
```

```java
//java entity class
public class Employee {  
  private int id;  
  private String name;  
  private float salary;  
}  
```

```java
public class EmployeeDao {  
private JdbcTemplate jdbcTemplate;  
  
public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {  
    this.jdbcTemplate = jdbcTemplate;  
}  
  
public int saveEmployee(Employee e){  
    String query="insert into employee values(  
    '"+e.getId()+"','"+e.getName()+"','"+e.getSalary()+"')";  
    return jdbcTemplate.update(query);  
}  
public int updateEmployee(Employee e){  
    String query="update employee set   
    name='"+e.getName()+"',salary='"+e.getSalary()+"' where id='"+e.getId()+"' ";  
    return jdbcTemplate.update(query);  
}  
public int deleteEmployee(Employee e){  
    String query="delete from employee where id='"+e.getId()+"' ";  
    return jdbcTemplate.update(query);  
}  
  
}  
```

The **DriverManagerDataSource** is used to contain the information about the database such as driver class name, connnection URL, username and password.

There is a property named **datasource** in the JdbcTemplate class of DriverManagerDataSource type. So, we need to provide the reference of DriverManagerDataSource object in the JdbcTemplate class for the datasource property.

```xml
<bean id="ds" class="org.springframework.jdbc.datasource.DriverManagerDataSource">  
<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver" />  
<property name="url" value="jdbc:oracle:thin:@localhost:1521:xe" />  
<property name="username" value="system" />  
<property name="password" value="oracle" />  
</bean>  
  
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">  
<property name="dataSource" ref="ds"></property>  
</bean>  
  
<bean id="edao" class="com.javatpoint.EmployeeDao">  
<property name="jdbcTemplate" ref="jdbcTemplate"></property>  
</bean>  
```

# PreparedStatements

We use prepared statments to execute parameterized query using Spring JdbcTemplate.

```java
public Boolean saveEmployeeByPreparedStatement(final Employee e){  
    String query="insert into employee values(?,?,?)";  
    
    return jdbcTemplate.execute(query,new PreparedStatementCallback<Boolean>(){  
    @Override  
    public Boolean doInPreparedStatement(PreparedStatement ps)  
            throws SQLException, DataAccessException {  
              
        ps.setInt(1,e.getId());  
        ps.setString(2,e.getName());  
        ps.setFloat(3,e.getSalary());  
              
        return ps.execute();  
              
    }  
    });  
```

### ResultSetExtractor

We can easily fetch the records from the database using **query()** method of **JdbcTemplate** class where we need to pass the instance of ResultSetExtractor.

**ResultSetExtractor** interface can be used to fetch records from the database. It accepts a ResultSet and returns the list.

```java
public class EmployeeDao {  

private JdbcTemplate template;  
  
public List<Employee> getAllEmployees(){  
 return template.query("select * from employee",
 
 new ResultSetExtractor<List<Employee>>(){  
    @Override  
     public List<Employee> extractData(ResultSet rs) throws SQLException,  
            DataAccessException {  
      
        List<Employee> list=new ArrayList<Employee>();  
        while(rs.next()){  
        Employee e=new Employee();  
        e.setId(rs.getInt(1));  
        e.setName(rs.getString(2));  
        e.setSalary(rs.getInt(3));  
        list.add(e);  
        }  
        return list;  
        }  
    });  
  }  
}  
```

### RowMapper Interface

**RowMapper** interface allows to map a row of the relations with the instance of user-defined class. It iterates the ResultSet internally and adds it into the collection. So we don't need to write a lot of code to fetch the records as **ResultSetExtractor**.

```java
public List<Employee> getAllEmployeesRowMapper(){  
 return template.query("select * from employee",
 
 new RowMapper<Employee>(){  
    @Override  
    public Employee mapRow(ResultSet rs, int rownumber) throws SQLException {  
        Employee e=new Employee();  
        e.setId(rs.getInt(1));  
        e.setName(rs.getString(2));  
        e.setSalary(rs.getInt(3));  
        return e;  
    }  
    });  
}  
```

# Spring NamedParameterJdbcTemplate

Spring provides another way to insert data by named parameter. In such way, we use names instead of ?(question mark).

```java

public class EmpDao {  

  NamedParameterJdbcTemplate template;

  public void save (Employee e){  
    String query="insert into employee values (:id,:name,:salary)";  
    
    Map<String,Object> map=new HashMap<String,Object>();  
    map.put("id",e.getId());  
    map.put("name",e.getName());  
    map.put("salary",e.getSalary());  
      
    template.execute(query,map,new PreparedStatementCallback() {  
      @Override  
      public Object doInPreparedStatement(PreparedStatement ps)  
              throws SQLException, DataAccessException {  
          return ps.executeUpdate();  
      }  
    });  
  }
}
```

# Spring SimpleJdbcTemplate

Spring 3 JDBC supports the java 5 feature var-args (variable argument) and autoboxing by the help of SimpleJdbcTemplate class.

SimpleJdbcTemplate class wraps the JdbcTemplate class and provides the update method where we can pass arbitrary number of arguments.

#### Syntax of update method of SimpleJdbcTemplate class
```java
int update(String sql,Object... parameters)
```

```java
public class EmpDao {  
  SimpleJdbcTemplate template;  
  
  public int update (Employee e){  
    String query="update employee set name=? where id=?";  
    return template.update(query,e.getName(),e.getId());  
  }

  public in update2 (Employee e){
    String query="update employee set name=?,salary=? where id=?";  
    return template.update(query,e.getName(),e.getSalary(),e.getId()); 
  }
}
```
