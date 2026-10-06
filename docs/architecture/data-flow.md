# Data Flow

## Source Topic

`orders.events.v1`

## Processing Outputs

- `orders.validated.v1`
- `orders.customer-metrics.v1`
- `orders.dlq.v1`

## Snowflake Layers

- `RAW`: Original source events and rejected records.
- `STAGING`: Typed and validated order lifecycle events.
- `MART`: Windowed customer order metrics.
- `MONITORING`: Reconciliation and operational results.
