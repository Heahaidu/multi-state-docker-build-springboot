# Overview
Multi-stage Dockerization of Spring Boot applications involves creating a Docker image that separates the build and runtime environments. This approach helps in reducing the final image size and improving security.

# Multi-Stage

## Stage 1 Download the dependencies

```
FROM eclipse-temurin:21-jdk AS dependencies

RUN apk add --no-cache maven

WORKDIR /app

COPY pom.xml .

RUN mvn dependency:go-offline 
```

## Stage 2 Build the application

```
FROM dependencies AS builder

COPY src ./src

RUN mvn clean package -DskipTests
```

## Stage 3 Run the application

```
FROM eclipse-temurin:21-jre AS runtime

WORKDIR /app

# Copy JAR from builder
COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
```

# RUN

``` bash
docker build -t springboot-health-bootstrap:1.0.2 -f Dockerfile-1.0.2 .
```
