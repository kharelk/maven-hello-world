# FROM openjdk:21-jdk-slim
# RUN groupadd -r appuser && useradd -r -g appuser appuser
# WORKDIR /app
# ARG ENV
# ENV APP_ENV=$ENV
# COPY ${ENV}/*.jar app.jar
# USER appuser
# ENTRYPOINT ["java", "-jar", "app.jar"]





# Stage 1: Build (only used when BUILD_IN_DOCKER=true)
FROM maven:3.9.9-amazoncorretto-21-debian AS build
ARG BUILD_IN_DOCKER
WORKDIR /build
COPY myapp/ .
RUN if [ "$BUILD_IN_DOCKER" = "true" ]; then \
      mvn -f myapp/pom.xml --batch-mode clean package; \
    fi

# Stage 2: Runtime stage
FROM openjdk:21-jdk-slim
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app

ARG BUILD_IN_DOCKER
ARG ENV
ENV APP_ENV=$ENV

RUN echo "Copying built artifact from the build stage (will override if BUILD_IN_DOCKER = true)..."
COPY --from=build /build/target/*.jar app.jar

RUN if [ "$BUILD_IN_DOCKER" = "true" ]; then \
      echo "Copying pre-built artifact from ${ENV}..." && \
      cp ${ENV}/*.jar app.jar; \
    fi

USER appuser
ENTRYPOINT ["java", "-jar", "app.jar"]