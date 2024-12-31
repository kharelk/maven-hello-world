# FROM openjdk:21-jdk-slim
# RUN groupadd -r appuser && useradd -r -g appuser appuser
# WORKDIR /app
# ARG ENV
# ENV APP_ENV=$ENV
# COPY ${ENV}/*.jar app.jar
# USER appuser
# ENTRYPOINT ["java", "-jar", "app.jar"]





# Stage 1: Build (only used when BUILD_IN_DOCKER=true)
FROM maven:3.8.8-openjdk-21-slim AS build
ARG BUILD_IN_DOCKER
WORKDIR /build
RUN if [ "$BUILD_IN_DOCKER" = "true" ]; then \
      COPY myapp/ . && \
      mvn -f myapp/pom.xml --batch-mode clean package \
    fi

# Stage 2: Runtime stage
FROM openjdk:21-jdk-slim
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app

ARG BUILD_IN_DOCKER
ARG ENV
ENV APP_ENV=$ENV

RUN if [ "$BUILD_IN_DOCKER" = "true" ]; then \
      echo "Copying built artifact from the build stage..." && \
      COPY --from=build /build/target/*.jar app.jar; \
    else \
      echo "Copying pre-built artifact from ${ENV}..." && \
      COPY ${ENV}/*.jar app.jar; \
    fi

USER appuser
ENTRYPOINT ["java", "-jar", "app.jar"]