tags : [[Spring ORM]], [[Hibernate Lifecycle]]

![[Pasted image 20240608121317.png]]

In hibernate framework, we provide all the database information hibernate.cfg.xml file.

But if we are going to integrate the hibernate application with spring, we don't need to create the hibernate.cfg.xml file. We can provide all the information in the applicationContext.xml file.

Hibernate without Spring:

```java
//creating configuration  
Configuration cfg=new Configuration();    
cfg.configure("hibernate.cfg.xml");    
    
//creating seession factory object    
SessionFactory factory=cfg.buildSessionFactory();    
    
//creating session object    
Session session=factory.openSession();    
    
//creating transaction object    
Transaction t=session.beginTransaction();    
        
Employee e1=new Employee(111,"arun",40000);    
session.persist(e1);//persisting the object    
    
t.commit();//transaction is commited    
session.close();
```

Hibernate with Spring : 

The Spring framework provides **HibernateTemplate** class, so you don't need to follow so many steps like create Configuration, BuildSessionFactory, Session, beginning and committing transaction etc.

```java
Employee e1=new Employee(111,"arun",40000);    
hibernateTemplate.save(e1);
```

