FROM openjdk:21-jdk-slim
RUN groupadd -r appuser && useradd -r -g appuser appuser
WORKDIR /app
ARG ENV
ENV APP_ENV=$ENV
COPY ${ENV}/*.jar app.jar
USER appuser
ENTRYPOINT ["java", "-jar", "app.jar"]
