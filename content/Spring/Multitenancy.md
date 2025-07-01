tags : [[Spring Boot]]

While the previous solution provides a detailed approach to achieve multi-tenancy with multiple data sources in Spring Boot, there are alternative approaches that can simplify the configuration. One such approach is using the `AbstractRoutingDataSource` class provided by Spring Framework.

Here are the steps to configure additional tenants and their data sources using `AbstractRoutingDataSource`:

1. **Add Maven Dependencies**: Add the necessary dependencies to your `pom.xml` file:
   - `spring-boot-starter-data-jpa` for Spring Data JPA support.
   - `postgresql` (or any other database driver) for the specific database you are using.

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-jpa</artifactId>
   </dependency>

   <dependency>
       <groupId>org.postgresql</groupId>
       <artifactId>postgresql</artifactId>
       <scope>runtime</scope>
   </dependency>
   ```

2. **Create Tenant Configuration**: Create a configuration class to hold the tenant-specific data source properties. Each tenant will have its own instance of this configuration class.

   ```java
   @ConfigurationProperties(prefix = "tenant")
   public class TenantProperties {
       private String url;
       private String username;
       private String password;
       // getters and setters
   }
   ```

3. **Configure Data Sources**: Create a bean for each tenant's data source using the `TenantProperties` configuration class. Use `DataSourceBuilder` to build the data source based on the properties.

   ```java
   @Configuration
   public class DataSourceConfig {

       @Bean
       @ConfigurationProperties(prefix = "spring.datasource")
       public DataSourceProperties dataSourceProperties() {
           return new DataSourceProperties();
       }

       @Bean
       @ConfigurationProperties(prefix = "tenant1.datasource")
       public TenantProperties tenant1Properties() {
           return new TenantProperties();
       }

       @Bean
       @ConfigurationProperties(prefix = "tenant2.datasource")
       public TenantProperties tenant2Properties() {
           return new TenantProperties();
       }

       @Bean
       public DataSource dataSource() {
           Map<Object, Object> targetDataSources = new HashMap<>();
           targetDataSources.put("tenant1", DataSourceBuilder.create()
                   .driverClassName(dataSourceProperties().getDriverClassName())
                   .url(tenant1Properties().getUrl())
                   .username(tenant1Properties().getUsername())
                   .password(tenant1Properties().getPassword())
                   .build());

           targetDataSources.put("tenant2", DataSourceBuilder.create()
                   .driverClassName(dataSourceProperties().getDriverClassName())
                   .url(tenant2Properties().getUrl())
                   .username(tenant2Properties().getUsername())
                   .password(tenant2Properties().getPassword())
                   .build());

           AbstractRoutingDataSource routingDataSource = new AbstractRoutingDataSource() {
               @Override
               protected Object determineCurrentLookupKey() {
                   // Implement the logic to determine the current tenant identifier
                   // This can be based on user authentication or any other logic
                   return "tenant1";
               }
           };

           routingDataSource.setTargetDataSources(targetDataSources);
           routingDataSource.setDefaultTargetDataSource(targetDataSources.get("tenant1"));
           routingDataSource.afterPropertiesSet();

           return routingDataSource;
       }
   }
   ```

4. **Configure JPA and Hibernate**: Configure JPA and Hibernate to use the multi-tenant data source. Set the `hibernate.multiTenancy` property to `DATABASE` and provide the `MultiTenantConnectionProvider` and `CurrentTenantIdentifierResolver` beans.

   ```java
   @Configuration
   public class JpaConfig {

       @Autowired
       private DataSource dataSource;

       @Bean
       public LocalContainerEntityManagerFactoryBean entityManagerFactory() {
           Map<String, Object> properties = new HashMap<>();
           properties.put("hibernate.multiTenancy", MultiTenancyStrategy.DATABASE);
           properties.put("hibernate.multi_tenant_connection_provider", multiTenantConnectionProvider());
           properties.put("hibernate.tenant_identifier_resolver", currentTenantIdentifierResolver());

           LocalContainerEntityManagerFactoryBean em = new LocalContainerEntityManagerFactoryBean();
           em.setDataSource(dataSource);
           em.setJpaVendorAdapter(new HibernateJpaVendorAdapter());
           em.setPackagesToScan("com.example.domain");
           em.setJpaPropertyMap(properties);
           return em;
       }

       @Bean
       public MultiTenantConnectionProvider multiTenantConnectionProvider() {
           return new DataSourceBasedMultiTenantConnectionProviderImpl();
       }

       @Bean
       public CurrentTenantIdentifierResolver currentTenantIdentifierResolver() {
           return new CurrentTenantIdentifierResolver() {
               @Override
               public String resolveCurrentTenantIdentifier() {
                   // Implement the logic to determine the current tenant identifier
                   // This can be based on user authentication or any other logic
                   return "tenant1";
               }

               @Override
               public boolean validateExistingCurrentSessions() {
                   return false;
               }
           };
       }
}```

https://www.baeldung.com/multitenancy-with-spring-data-jpa

**when we perform any tenant operation, we need to know the tenant ID before creating any transaction**. So, we need to set the tenant ID in a [_Filter_](https://www.baeldung.com/spring-boot-add-filter) or [_Interceptor_](https://www.baeldung.com/spring-mvc-handlerinterceptor) before hitting controller endpoints.
