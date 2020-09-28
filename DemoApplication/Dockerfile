# Springboot sample application

FROM openjdk:11-jdk-slim

LABEL maintainer="gowda.shivakumar@gmail.com"

VOLUME /tmp

EXPOSE 9080 9085

ARG JAR_FILE=/target/DemoPractice-0.0.1-SNAPSHOT.jar

ADD ${JAR_FILE} demopractice.jar

ENTRYPOINT ["java","-jar", "/demopractice.jar"]
