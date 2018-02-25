FROM maven:alpine AS builder

ADD . /traccar
WORKDIR /traccar

RUN mvn package


FROM java:8-alpine

MAINTAINER Danilo Recchia <danilo.recchia@vortus.solutions>

USER root

RUN apk upgrade --update && \
    apk add --update curl bash && \
    rm -rf /var/cache/apk/* && \
    mkdir -p /opt/traccar/logs && \
    mkdir -p /opt/traccar/data

ENV JAVA_OPTS -Xms256m -Xmx1024m

COPY ./setup/traccar.xml /opt/traccar/traccar.xml
COPY ./setup/default.xml /opt/traccar/conf/default.xml
COPY ./schema /opt/traccar/schema
COPY ./templates /opt/traccar/templates
COPY ./traccar-web/web /opt/traccar/web

COPY --from=builder /traccar/target/tracker-server.jar /opt/traccar/traccar-server.jar
COPY --from=builder /traccar/target/lib /opt/traccar/lib

EXPOSE 8082
EXPOSE 5000-5150

WORKDIR /opt/traccar

ENTRYPOINT ["java","-jar","traccar-server.jar","traccar.xml"]
