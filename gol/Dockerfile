FROM tomcat:9.0.73-jdk8-corretto-al2
ARG HOME_DIR=/tomcat/webapps
ADD https://referenceapplicationskhaja.s3.us-west-2.amazonaws.com/gameoflife.war ${HOME_DIR}/gameoflife.war
WORKDIR ${HOME_DIR}
EXPOSE 8080
CMD [ "java","-jar","gameoflife.war" ]




# game of life
FROM alpine/git AS VCS
RUN cd / && git clone https://github.com/wakaleo/game-of-life.git

FROM maven:3-amazoncorretto-8 AS builder
COPY --from=VCS /game-of-life /game-of-life
RUN cd /game-of-life && mvn package

FROM tomcat:9.0.73-jdk8-corretto-al2
ARG HOME_DIR=/tomcat/webapps
COPY --from=builder /game-of-life/gameoflife-web/target/gameoflife.war ${HOME_DIR}/gameoflife.war
WORKDIR ${HOME_DIR}
EXPOSE 8080
CMD ["java","-jar","gameoflife.war"]
