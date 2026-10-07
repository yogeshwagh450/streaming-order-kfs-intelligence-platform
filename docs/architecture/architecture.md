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


## System Boundaries

### In Scope for Level 1

- Confluent Cloud Kafka as the managed event-streaming platform.
- Confluent Schema Registry for Avro schema governance.
- Python-based order event producer and diagnostic consumer.
- Apache Flink and PyFlink for stateful stream processing.
- Kafka output topics for validated events, aggregates, and rejected events.
- Managed Snowflake sink connectors for warehouse ingestion.
- Snowflake `RAW`, `STAGING`, `MART`, and `MONITORING` layers.

### Out of Scope for Level 1

- Multi-region Kafka replication.
- Change data capture platforms.
- Multi-cloud disaster recovery.
- Real-time feature stores.
- AI-based operational remediation.

## Delivery Boundaries

The source topic has two independent downstream paths:

1. Original source events are delivered to Snowflake `RAW` for auditability.
2. Apache Flink consumes the same source topic for validation, stateful
   processing, event-time aggregation, and DLQ routing.

Flink publishes results back to Kafka output topics. Managed Snowflake sink
connectors then deliver those output topics to their target Snowflake layers.

## Processing Guarantees

Processing and delivery guarantees are documented independently for:

- Python producer to Kafka.
- Kafka to Flink stateful processing.
- Flink output to Kafka.
- Kafka connector delivery to Snowflake.

The project will not claim end-to-end exactly-once behavior until the guarantee
at every boundary has been configured and verified through failure testing.