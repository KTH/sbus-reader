# sbus-reader

Application which reads events from a queue and writes them to a folder.

## Configuration

The application starts with the class path `/run/secrets:/opt/data:/opt/camel`, and
looks for a file named camel.properties for configuration. This allows for a number
of options on how to configure the application.

1. You can use environment variables, which is used to create camel.properties in /opt/camel
   when the container starts. There is a sceleton file in environment.in.
1. You can instead choose to use properties in either /opt/data, which can be made a volume
   mount to some shared area. There is a sceleton file in camel.properties.in.
1. Or you can use properties files in /run/secrets, using the new mechanism for secrets in Docker
   1.13.

There are no defaults, all options must be set regardless of method. Methods cannot
be combined.

### Service Bus configuration variables

| Variable | Property | Description |
|----------|----------|---------------------|
| SERVICE_BUS_URI | service_bus.uri | URI to Service Bus instance, without protocol, e.g: kth-integral.servicebus.windows.net |
| SERVICE_BUS_USER | service_bus.user | The Service Bus principal ID, needs write access to the entire instance |
| SERVICE_BUS_PASSWORD | service_bus.password | The Service Bus key for the principal ID |
| SERVICE_BUS_QUEUE | service_bus.queue | The Service Bus queue to read from |

## Running

Given a file environment containing environment variables as mentioned above, the image can be run with docker run as in this example.

```
docker run --name=sbus-reader \
    --env-file=environment \
    -v /opt/camel /opt/camel \
    kthse/sbus-reader:latest
```

## Development

There is no source code here, it's just a packaging and configuration of off-the-shelf
Apache Camel components. So this is about Camel and nothing else currently.

## Building

Currently the build depends on libraries that are not available in public
repositories.

A complete build of the image can be made with `mvn clean package docker:build`.

Due to the need to access a private maven repository, you may be better off building
the image from the integral-ug-multicast Jenkins project which has access to the
private repository. Pre built images are available on public docker hub as
kthse/sbus-reader, see above.
