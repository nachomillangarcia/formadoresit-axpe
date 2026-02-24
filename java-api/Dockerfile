FROM maven:3.9.6-eclipse-temurin-17

WORKDIR /app

COPY pom.xml .

RUN mvn dependency:go-offline

COPY src ./src

RUN mvn package && \
    cp target/java-server-*-jar-with-dependencies.jar app.jar

ENTRYPOINT ["java", "-jar", "app.jar"]

