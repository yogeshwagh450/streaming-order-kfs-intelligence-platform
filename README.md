# OrderStream KFS Platform

Real-time order event streaming platform built on **K**afka (Confluent Cloud),
**F**link and **S**nowflake, with Python producers and schema-governed Avro events.

## Project Objectives

- Produce schema-governed order events to Confluent Cloud Kafka.
- Process events using Apache Flink and event-time semantics.
- Validate, deduplicate and enrich order lifecycle events.
- Calculate real-time customer metrics using windowed aggregation.
- Route invalid records to a dead-letter topic.
- Deliver raw, validated and aggregated streams to Snowflake.
- Demonstrate checkpoint recovery and production failure scenarios.

## Architecture

```text
Python Producer
      |
      v
Confluent Cloud Kafka (orders.events.v1)
      |
      +-------> Snowflake RAW
      |
      v
Apache Flink
      |
      +-------> orders.validated.v1
      +-------> orders.customer-metrics.v1
      +-------> orders.dlq.v1
                         |
                         v
                  Snowflake Sink
                         |
          STAGING / MART / RAW DLQ
```

## Primary Technologies

- Python
- Confluent Cloud Kafka
- Confluent Schema Registry
- Apache Avro
- Apache Flink and PyFlink
- Snowflake
- GitHub Actions

## Project Status

Milestone 0: Architecture and environment bootstrap.

## Security

Credentials, API keys, private keys and local property files must not be
committed. Use `.env.example` and `*.example.properties` files only.

## Documentation

Architecture decisions are maintained in `docs/architecture/decisions/`.
