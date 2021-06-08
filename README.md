# Bytehoops
Exchange messages between HTTP endpoints and Kafka while retaining headers

Bytehoops is an application that can run in two modes:
1. Producer: In this mode, headers and payload of a request POSTed to an HTTP endpoint is forwarded to an Apache Kafka topic
2. Consumer: Every message consumed from Apache Kafka is POSTed to an HTTP endpoint. The Message Headers of Kafka Message are retained as HTTP headers

This motivation for this project was to create a way to push data to a Prometheus remote storage via Apache Kafka for added reliability. 
## Quickstart
### Producer Mode
All messages POSTed to `http://localhost:8080/api/v1/write` will end up in the Kafka topic `prometheus`
```shell
$ docker run ghcr.io/tuxiedev/bytehoops producer
```

### Consumer Mode
All messages consumed from a Kafka topic `prometheus` will be  posted to an http endpoint `http://localhost:9009/api/v1/push`
```shell
$ docker run ghcr.io/tuxiedev/bytehoops consumer
```

For a complete integration example, check out the [prompush](https://github.com/tuxiedev/prompush) project. Specifically, you can check out the [docker-compose.yml](https://github.com/tuxiedev/prompush/blob/master/docker-compose.yml) on how to set up and end to end pipeline with bytehoops.

## Command Reference
### Producer Mode
```shell
$ ./bytehoops producer --help
Produces a byte array published to this http endpoint to Kafka

Usage:
  bytehoops producer [flags]

Flags:
  -h, --help                    help for producer
  -l, --listen-address string   Listening address for the API server to receive events (default ":8080")

Global Flags:
  -b, --bootstrap-servers strings   Comma separated list of bootstrap servers (default [localhost:9092])
  -t, --topic string                Topic to consume or send data to (default "prometheus")
```

### Consumer Mode
```shell
$ ./bytehoops consumer --help
Consumes byte arrays from Kafka and posts messages to an API endpoint

Usage:
  bytehoops consumer [flags]

Flags:
  -g, --group-name string      Name of the Kafka consumer group (default "bytehoops")
  -h, --help                   help for consumer
  -s, --sink-endpoint string   Path to remote http endpoint (default "http://localhost:9009/api/v1/push")

Global Flags:
  -b, --bootstrap-servers strings   Comma separated list of bootstrap servers (default [localhost:9092])
  -t, --topic string                Topic to consume or send data to (default "prometheus")
```

