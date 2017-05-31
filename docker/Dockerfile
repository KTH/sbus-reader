FROM openjdk:8-jre-alpine

RUN apk --no-cache add bash

# Service bus configuration
# ENV SERVICE_BUS_URI the-service-bus-uri
# ENV SERVICE_BUS_USER the-service-bus-user
# ENV SERVICE_BUS_PASSWORD the-service-bus-password
# ENV SERVICE_BUS_QUEUE the-service-bus-queue

# Jolokia management and discovery
EXPOSE 8181 24884

ADD sbus-reader-*.jar /opt/camel/application.jar
ADD run.sh /opt/integral/run.sh
RUN chmod +x /opt/integral/run.sh

WORKDIR /opt/camel
ENTRYPOINT ["/opt/integral/run.sh"]
CMD ["start"]
