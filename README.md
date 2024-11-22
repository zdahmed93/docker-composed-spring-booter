# Spring Boot PostgreSQL Docker Compose Development Template

A streamlined development template for Spring Boot applications with PostgreSQL, running entirely in Docker containers. No local Java or Maven installation required!

## 🚀 Features

- Ready-to-use Spring Boot development environment
- PostgreSQL database configuration
- Docker-based development workflow
- Hot-reload support
- Volume-based dependency caching
- Persistent database storage
- Configurable through environment variables

## 📋 Prerequisites

- Docker Desktop
- IDE (VS Code, IntelliJ IDEA, etc.)
- Docker extension for your IDE (recommended)

## 🏃‍♂️ Getting Started

### 1. Clone the Template

```bash
git clone [repository-url]
cd [project-name]
```

### 2. Start the Application

```bash
docker-compose up --build
```

Your application will be available at:

- Application: http://localhost:8080
- Database: localhost:5436

### 3. Stop the Application

```bash
docker-compose down
```

## 💻 Development Workflow

### Adding New Dependencies

1. Add the dependency to `pom.xml`:

```xml
<dependency>
  <groupId>group-id</groupId>
  <artifactId>artifact-id</artifactId>
  <version>version</version>
</dependency>
```

2. Rebuild the containers:

```bash
docker-compose up --build
```

### Creating New Endpoints

1. Create a new controller class in `src/main/java/com/zdahmed93/starter/controllers`:

```java
@RestController
@RequestMapping("/api")
public class MyController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

2. The changes will be automatically detected and reloaded.

### Adding Database Entities

1. Create a new entity class in `src/main/java/com/zdahmed93/starter/entities`:

```java
@Entity
@Table(name = "my_table")
public class MyEntity {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
// Add fields
}
```

2. Create corresponding repository in `src/main/java/com/zdahmed93/starter/repositories`:

```java
@Repository
public interface MyRepository extends JpaRepository<MyEntity, Long> {
}
```

### Running Tests

```bash
docker-compose run app mvn test
```

## 🔧 Configuration

### Environment Variables

Configure these in `docker-compose.yml`:

```yaml
environment:
SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/mydatabase
SPRING_DATASOURCE_USERNAME=postgres
SPRING_DATASOURCE_PASSWORD=postgres
```

## Application Properties

Modify `src/main/resources/application.properties`:

### Server Configuration

```properties
server.port=8080
```

### Database Configuration

```properties
spring.datasource.url=jdbc:postgresql://db:5432/mydatabase
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.jpa.hibernate.ddl-auto=update
```

## 📚 Common Docker Commands

View logs

```bash
docker-compose logs -f
```

Rebuild containers

```bash
docker-compose up --build
```

Stop and remove containers

```bash
docker-compose down
```

Remove volumes (clean database)

```bash
docker-compose down -v
```

Run specific service

```bash
docker-compose run app [command]
```

Shell into container

```bash
docker-compose exec app sh
```

## 🔍 Debugging

### IDE Configuration

#### VS Code

1. Install Docker extension
2. Add launch configuration:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "java",
      "name": "Debug Spring Boot",
      "request": "attach",
      "hostName": "localhost",
      "port": 5005
    }
  ]
}
```

## 📦 Building for Production

1. Create production Dockerfile:

```dockerfile
FROM eclipse-temurin:17-jdk-alpine as build
WORKDIR /app
COPY . .
RUN ./mvnw clean package -DskipTests
FROM eclipse-temurin:17-jre-alpine
COPY --from=build /app/target/.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

2. Build production image:

```bash
docker build -t myapp:1.0 .
```
