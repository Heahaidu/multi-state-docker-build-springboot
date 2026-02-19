# Overview
Multi-stage Dockerization of Spring Boot applications involves creating a Docker image that separates the build and runtime environments. This approach helps in reducing the final image size and improving security.

# Result

| Version | Build Time | Image Size | Base Runtime Image | Health Check | 
|---------|------------|------------|--------------------|--------------|
| 1.0.1   | ~1s        | 370MB      | eclipse-temurin:21-jre-alpine | curl | 
| 1.0.2   | ~1s        | 320MB      | eclipse-temurin:21-jre-alpine | wget |
| 1.0.3   | ~2s        | 155MB      | alpine:3.21        | wget         | 