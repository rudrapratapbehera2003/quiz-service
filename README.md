# Quiz Service

The **Quiz Service** handles quiz creation, user attempts, and scoring. It communicates with a MySQL database for persistence and utilizes **Spring Cloud OpenFeign** to dynamically communicate with the `question-service` via service discovery.

## Available Features / Endpoints
- Create new quizzes based on category and number of questions
- Fetch quiz details and evaluate answers
- Service-to-service communication via OpenFeign

## Procedure To Run The Application Locally

Take a pull from the repository and let the application resolve the dependencies on the basis of pom.xml file.

### Steps To Successfully Run The Application

1. **Ensure Service Registry & Question Service Are Running** so that Feign client can resolve dependencies properly.
2. **Configure MySQL Database** with the proper credentials.
3. **Run the Spring Boot Application** locally via your IDE or using Maven (`mvn spring-boot:run`).

## Containerizing The Service (Using Docker)

1. **Ensure Shared Network Exists**
    ```bash
    docker network create microservices-net
    ```

2. **Run MySQL Container**
    ```bash
    docker run -d \
    --name mysql-db \
    --network microservices-net \
    -p 3307:3306 \
    -e MYSQL_DATABASE=quiz \
    -e MYSQL_ROOT_PASSWORD=your_password_here\
    mysql:8.3
    ```

3. **Build & Run Quiz Service Container**
    ```bash
    docker build -t quiz-service:latest .

    docker run -d \
    --name quiz-service \
    --network microservices-net \
    -p 8082:8082 \
    --env-file .env \
    -e EUREKA_CLIENT_SERVICE_URL_DEFAULTZONE=http://registry-service:8761/eureka/ \
    quiz-service:latest
    ```