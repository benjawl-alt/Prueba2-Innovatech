# Etapa 1: Compilación (Build)
FROM maven:3.8.5-openjdk-17-slim AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Imagen Final (Runtime)
FROM amazoncorretto:17-alpine
WORKDIR /app

# Seguridad: Ejecución con usuario no root
RUN addgroup --system spring && adduser --system spring --ingroup spring
USER spring:spring

COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
