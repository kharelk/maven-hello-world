FROM debian:bookworm-slim AS build
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*


FROM openjdk:21-jdk-slim
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app
ARG ENV
ENV APP_ENV=$ENV
COPY ${ENV}/*.jar app.jar
COPY --from=build /usr/bin/curl /usr/bin/curl
USER appuser
HEALTHCHECK --interval=30s --timeout=10s \
  CMD curl --silent --fail http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["java", "-jar", "app.jar"]