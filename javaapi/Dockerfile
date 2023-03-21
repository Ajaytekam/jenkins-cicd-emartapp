FROM openjdk:8

COPY ./target/book-work-0.0.1-SNAPSHOT.jar /usr/src/app/book-work-0.0.1.jar
WORKDIR /usr/src/app/

EXPOSE 9000
ENTRYPOINT ["java","-jar","book-work-0.0.1.jar"]
