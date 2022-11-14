FROM openjdk:8-jdk-alpine

ARG JAR_FILE

COPY target/${JAR_FILE} gateway-server.jar

ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/gateway-server.jar"]
