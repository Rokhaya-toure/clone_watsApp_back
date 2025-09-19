# Étape 1 : build
FROM eclipse-temurin:21-jdk-alpine AS build

# Crée le répertoire de travail
WORKDIR /app

# Copie les fichiers Maven et le wrapper
COPY pom.xml mvnw ./
COPY .mvn .mvn

# Donne les droits d'exécution au wrapper Maven (important pour Alpine)
RUN chmod +x mvnw

# Télécharge les dépendances Maven (cache pour accélérer les builds)
RUN ./mvnw dependency:go-offline

# Copie le code source
COPY src ./src

# Compile et package le projet
RUN ./mvnw clean package -DskipTests

# Étape 2 : image finale
FROM eclipse-temurin:21-jre-alpine

# Répertoire de travail
WORKDIR /app

# Copier le JAR depuis l'étape de build
COPY --from=build /app/target/*.jar app.jar

# Expose le port de l'application
EXPOSE 8080

# Commande pour démarrer l'application
ENTRYPOINT ["java", "-jar", "app.jar"]
