# API Gateway HTTP <> Kafka

First start Kafka

```shell
docker-compose -f kafka.docker-compose.yml up -d
```

Start RIG locally:

```shell
cd ../../../
export KAFKA_SOURCE_TOPICS=example
export KAFKA_BROKERS=localhost:9092
config=$(cat examples/api-gateway/kafka/config.json)
export PROXY_CONFIG_FILE="$config"
mix run --no-halt
```

[Download some quickstart scripts for Kafka](https://kafka.apache.org/quickstart) and run a consumer (note: there must not be a space in the path of the script):

```shell
/path/to/download/folder/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic example --from-beginning
```

After that we should be able to send events to Kafka through RIG:

```shell
curl -X POST \
  -H 'traceparent: 00-9c18b63f316cbfe3854122c20c8c6b23-22d69e0945046f2d-01' \
  --data '{"specversion":"0.2","type":"com.github.pull.create","source":"https://github.com/cloudevents/spec/pull","id":"A234-1234-1234","time":"2018-04-05T17:31:00Z","data":"hello"}' \
  localhost:4000
```

You should see the message in the Kafka consumer.

> RIG does not yet support forwarding trace context to a Kafka backend. There is an issue that addresses this.
