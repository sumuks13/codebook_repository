
### **What is Docker?**

- Docker is a **tool that lets you package applications with all their dependencies** into a single unit called a **container**. 
- These containers run consistently across any environment—your laptop, a server or the cloud.

Think of a container as a **lightweight, portable virtual machine**, but without the overhead of a full operating system.

### **Explain the basic Docker terminologies**

| Term           | Meaning                                                                 |
| -------------- | ----------------------------------------------------------------------- |
| **Image**      | A snapshot of your application and its dependencies (like a blueprint). |
| **Container**  | A running instance of an image.                                         |
| **Dockerfile** | A script used to build Docker images.                                   |
| **Docker Hub** | Public registry to find or store Docker images.                         |
| **Volume**     | Used to persist data from containers.                                   |
| **Port**       | Containers expose ports to communicate with the outside world.          |

### **What are some of the basic Docker commands?**

1. **Check Docker is Installed**

```bash
docker --version
```

2. **Download an Image**

```bash
docker pull nginx
```

3. **Run a Container**

```bash
docker run -d -p 8080:80 nginx
```

- `-d` = detached mode (runs in background) 
- `-p` = maps host port 8080 to container port 80

4. **List Running Containers**

```bash
docker ps
```

- To list **all containers** (running or stopped):

```bash
docker ps -a
```

5. **Stop a Container**

```bash
docker stop <container_id_or_name>
```

6. **Remove a Container**

```bash
docker rm <container_id_or_name>
```

7. **Remove an Image**

```bash
docker rmi <image_id_or_name>
```

8. **Build Image from Dockerfile**

```bash
docker build -t myapp:1.0 .
```

- `-t` = tag name (e.g., `myapp:1.0`)    
- `.` = current directory

9. **Run Image as Container**

```bash
docker run -it myapp:1.0
```

- `-it` = interactive terminal 

10. **List Images**

```bash
docker images
```

11. **Use Volumes (Persistent Data)**

```bash
docker run -v /my/local/path:/container/path myapp
```

12. **See Logs of a Container**

```bash
docker logs <container_id_or_name>
```

### **What is a Dockerfile?**

A **Dockerfile** is a **text file** that contains **a set of instructions** to build a **Docker image**.

A **Dockerfile** is a **text file** that contains **a set of instructions** to build a **Docker image** — which is basically a packaged environment with everything your application needs to run: code, libraries, dependencies, configurations.

Example: Simple Spring Boot App Dockerfile

```dockerfile
# Use base image with JDK
FROM eclipse-temurin:17-jdk-alpine

# Copy your jar file into the container
COPY target/my-app.jar app.jar

# Expose port (optional)
EXPOSE 8080

# Run the app
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### **What are some of the common Dockerfile commands?**

|Command|Purpose|
|---|---|
|`FROM`|**Base image** to start with (e.g., Java, Python, Alpine Linux)|
|`COPY`|Copy files from host machine into image|
|`ADD`|Like `COPY` but supports remote URLs and archive extraction|
|`RUN`|Run shell commands (e.g., install packages) during build|
|`CMD`|Default command to run when container starts|
|`ENTRYPOINT`|Like CMD but more strict — can’t be overridden easily|
|`WORKDIR`|Set working directory inside container|
|`EXPOSE`|Document which port the container listens on|
|`ENV`|Set environment variables|
|`ARG`|Define build-time variables|
|`VOLUME`|Declare mount point for volumes|
|`LABEL`|Metadata for the image (like version, maintainer info)|
|`USER`|Switch to a different user inside the container|
|`HEALTHCHECK`|Define how to check if container is healthy|
