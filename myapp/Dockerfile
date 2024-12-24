FROM openjdk:21-jdk-slim

# Create a non-root user
RUN groupadd -r appuser && useradd -r -g appuser appuser

# Set working directory and copy JAR file
WORKDIR /app
COPY stage/*.jar app.jar

# Set the user to non-root
USER appuser

# Run the JAR file
ENTRYPOINT ["java", "-jar", "app.jar"]
