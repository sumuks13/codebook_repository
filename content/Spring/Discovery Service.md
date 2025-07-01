tags: [[Spring Boot]], [[Microservices]], [[Load Balancing]]

Here are the key components and concepts associated with service discovery:

**1. Service Registry**

- A central database or registry that keeps track of information about available services in the system.
- Each microservice, when it starts or shuts down, registers or deregisters itself with the service registry. This includes information such as its network location, IP address, and the ports it is using.

**2. Service Provider**

- The microservice that offers a specific functionality or resource is considered a service provider.
- Service providers register themselves with the service registry so that other services can discover and connect to them.

**3. Service Consumer**

- The microservice that needs to use the functionality provided by another service is considered a service consumer.
- Service consumers query the service registry to discover the location and details of the service providers they want to interact with.

**4. Dynamic Updates**

- Service discovery systems handle dynamic updates seamlessly. As new microservices are added or existing ones are removed or scaled, the service registry is updated accordingly.
- This dynamic nature allows the system to adapt to changes without manual intervention.

**5. Load Balancing**

- Service discovery often involves load balancing mechanisms. When a service has multiple instances, load balancing ensures that requests are distributed optimally across these instances, improving performance and resource utilization.

**6. Decentralized Communication**

- Unlike traditional monolithic architectures where communication may be centralized, service discovery enables decentralized communication among microservices. Services can find and communicate with each other without relying on a central authority.

**7. Health Checking**

- Service discovery systems often include health checking mechanisms to monitor the status of service instances. Unhealthy or unreachable instances can be automatically excluded from the registry to ensure that only healthy services are used.

https://medium.com/@vishnuganb/eureka-server-the-ultimate-service-registry-for-microservices-359af8013dc3