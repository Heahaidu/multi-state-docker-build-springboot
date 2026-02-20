# Overview
Multi-stage Dockerization of Spring Boot applications involves creating a Docker image that separates the build and runtime environments. This approach helps in reducing the final image size and improving security.

# Result

| Version | Method | Build Time | Image Size | Base Runtime Image | Health Check |
|---------|--------|------------|------------|--------------------|--------------|
| 1.0.1   | Basic Multi-stage | ~1s        | 370MB      | eclipse-temurin:21-jre-alpine | curl |
| 1.0.2   | Optimized Health Check | ~1s        | 320MB      | eclipse-temurin:21-jre-alpine | wget |
| 1.0.3   | Layer Extraction | ~2s        | 155MB      | alpine:3.21 | wget |

---

## Version Details

### v1.0.1 - Basic Multi-stage
- Used `eclipse-temurin:21-jre-alpine` as the base runtime image, which includes a pre-installed JRE — no manual Java setup required.
- Health check implemented using `curl`.

### v1.0.2 - Optimized Health Check
- Replaced `curl` with `wget` for the health check, reducing the number of installed packages and slightly decreasing image size from **370MB → 320MB**.

### v1.0.3 - Layer Extraction
- Switched the base runtime image from `eclipse-temurin:21-jre-alpine` to `alpine:3.21` (a minimal Linux image with no JRE pre-installed), which significantly reduced image size from **320MB → 155MB**.
- Used **Spring Boot Layer Tools** to extract the application into separate layers (dependencies, snapshot-dependencies, resources, application), enabling more efficient Docker layer caching.
- Required manual installation of a JRE via `apk` in the Dockerfile.
- Build time increased slightly (~2s) due to the layer extraction step.

> ⚠️ **Note:** To enable layer extraction in v1.0.3, the following plugin configuration must be added to `pom.xml`:
> ```xml
> <plugin>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-maven-plugin</artifactId>
>     <configuration>
>         <!-- Separate layers for more efficient copying. -->
>         <layers>
>             <enabled>true</enabled>
>         </layers>
>     </configuration>
> </plugin>
> ```