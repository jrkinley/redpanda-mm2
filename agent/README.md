# Redpanda + Kafka + Forwarding Agent

As an alternative to using MirrorMaker2, use a lightweight, compiled agent that is deployed alongside Redpanda in the supermarket, to mirror the point-of-sale "agent-pos" topic to the central Kafka cluster.

## Download the agent...

...or download the source and build it for an alternate platform. It's written in Go, so it's easy to compile for the platform of your choice.
https://github.com/redpanda-data/redpanda-edge-plugin/releases

## Start the agent

```bash
./redpanda-edge-agent -config agent.yaml -loglevel=DEBUG
```

## Generate point-of-sale data

Generate random point-of-sale data in the supermarket (Redpanda) and consume it from the central cluster in the data centre (Kafka)

```bash
# Produce data to the "agent-pos" topic in Redpanda
for i in {1..60}; do echo "$RANDOM" |  rpk topic produce agent-pos --brokers ${REDPANDA_BROKERS}; sleep 1; done
```

```bash
# Consume data from the "agent-pos" topic in Kafka
rpk topic consume agent-pos --brokers ${KAFKA_BROKERS}
```
