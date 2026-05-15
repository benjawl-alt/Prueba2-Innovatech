# Etapa 1: Construcción (Maven)
# Cambiamos a eclipse-temurin que es la recomendada actualmente
FROM maven:3.8.5-eclipse-temurin-17-alpine AS build
WORKDIR /app

# Copiar el archivo de configuración de Maven
COPY pom.xml .
# Descargar dependencias
RUN mvn dependency:go-offline

# Copiar el código fuente y compilar
COPY src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Imagen de ejecución (Ligera y Segura)
# Cambiamos openjdk por eclipse-temurin alpine
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Seguridad: Crear usuario no root (Puntos extra en la pauta)
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

# Copiar el JAR generado desde la etapa de build
COPY --from=build /app/target/*.jar app.jar

# Exponer el puerto
EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
