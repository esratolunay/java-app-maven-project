FROM maven:3.9.4-eclipse-temurin-8-alpine
COPY ./java-app /app
WORKDIR /app
RUN mvn clean package

FROM openjdk:11-jre-slim
WORKDIR /app
COPY --from=0 /app/target/jb-hello-world-maven-0.2.0.jar /app/app.jar
CMD ["java","-jar","app.jar"]
