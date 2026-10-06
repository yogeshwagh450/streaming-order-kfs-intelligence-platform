# System Architecture

## Status

Milestone 0 architecture baseline.

## Primary Data Flow

1. A Python producer publishes Avro order events to Confluent Cloud Kafka.
2. Original source events are delivered independently to Snowflake RAW.
3. Apache Flink consumes the source event topic.
4. Flink validates, deduplicates and processes order lifecycle state.
5. Valid events, aggregate metrics and rejected events are published to
   separate Kafka topics.
6. Managed Snowflake sink connectors deliver Kafka records to the
   appropriate Snowflake data layers.

## Processing Keys

- `order_id`: Kafka partition key and order-lifecycle state key.
- `event_id`: Event deduplication identifier.
- `customer_id`: Customer aggregate processing key.
