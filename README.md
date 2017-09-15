# sbus-reader

A Docker container which reads events from a Service Bus queue using AMQP (Apache Qpid) and
writes them to a folder. The folder is /opt/data/events and data there can be accessed by
mounting a volume to that path as per usual with docker.

## Configuration

The application starts with the class path `/run/secrets:/opt/data:/opt/camel`, and
looks for a file named camel.properties for configuration. This allows for a number
of options on how to configure the application.

1. You can use environment variables, which is used to create env.properties in /opt/camel
   when the container starts.
1. You can instead choose to use properties in either /opt/data/camel.properties, which can be 
   made a volume mount to some shared area. There is a skeleton file in camel.properties.in.
1. Or you can use properties files in /run/secrets/secret.properties, using the new mechanism for 
   secrets in Docker 1.13.

There are no defaults, all options must be set regardless of method. Methods cannot
be combined.

### Service Bus configuration variables

| Variable | Property | Description |
|----------|----------|---------------------|
| SERVICE_BUS_URI | service_bus.uri | URI to Service Bus instance, without protocol, e.g: kth-integral.servicebus.windows.net |
| SERVICE_BUS_USER | service_bus.user | The Service Bus principal ID, needs write access to the entire instance |
| SERVICE_BUS_PASSWORD | service_bus.password | The Service Bus key for the principal ID |
| SERVICE_BUS_QUEUE | service_bus.queue | The Service Bus queue to read from |

### Other configuration variables

| Variable | Property | Description |
|----------|----------|---------------------|
| DIRECTORY | sbus_reader.directory | The name of the subfolder of /opt/data to write to, default 'events'.

## Running

Given a file environment containing environment variables as mentioned above, the image can be run with docker run as in this example.

```
docker run --name=sbus-reader \
    --env-file=environment \
    kthse/sbus-reader:latest
```

## Development

There is no source code here, it's just a packaging and configuration of off-the-shelf
Apache Camel components. So this is about Camel and nothing else currently.

## Building

A complete build of the image can be made with `mvn clean package docker:build`.
Pre built images are available on public docker hub as kthse/sbus-reader, see above.
