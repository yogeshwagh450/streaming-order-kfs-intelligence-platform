# Data Flow

## Source Topic

### `orders.events.v1`

- Key: `order_id`
- Producer: Python order producer
- Consumers:
  - Apache Flink order-processing job
  - Snowflake RAW sink connector
- Purpose: Canonical order lifecycle event stream

## Processing Output Topics

### `orders.validated.v1`

- Key: `order_id`
- Producer: Apache Flink
- Consumer: Snowflake validated-order sink connector
- Target layer: `STAGING`
- Purpose: Validated and deduplicated order lifecycle events

### `orders.customer-metrics.v1`

- Key: `customer_id`
- Producer: Apache Flink
- Consumer: Snowflake metrics sink connector
- Target layer: `MART`
- Purpose: Event-time windowed customer order metrics

### `orders.dlq.v1`

- Key: Original Kafka record key when available
- Producer: Apache Flink
- Consumer: Snowflake DLQ sink connector
- Target layer: `RAW`
- Purpose: Invalid or unprocessable events with failure context

## Snowflake Layers

- `RAW`: Original source events and rejected records.
- `STAGING`: Typed, validated, and deduplicated order lifecycle events.
- `MART`: Windowed customer order metrics.
- `MONITORING`: Reconciliation, data-quality, and operational results.