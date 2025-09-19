# Étape 1 : build
FROM eclipse-temurin:21-jdk-alpine AS build

# Crée le répertoire de travail
WORKDIR /app

# Copie les fichiers Maven
COPY pom.xml mvnw ./
COPY .mvn .mvn

# Télécharge les dépendances Maven (cache pour accélérer le build)
RUN ./mvnw dependency:go-offline

# Copie le code source
COPY src ./src

# Compile et package le projet
RUN ./mvnw clean package -DskipTests

# Étape 2 : image finale
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Copier le JAR depuis l'étape de build
COPY --from=build /app/target/*.jar app.jar

# Expose le port de l'application
EXPOSE 8080

# Commande pour démarrer l'application
ENTRYPOINT ["java","-jar","app.jar"]
