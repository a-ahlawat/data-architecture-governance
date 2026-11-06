# Order-line data contract

**Product** · Booked Order Lines  
**Contract version** · 1.0  
**Accountable owner** · Order Analytics Product Owner  
**Source stewards** · Order Operations and Customer Data  
**Consumers** · Finance Analytics and Commercial Reporting

One record represents one booked order line. The composite key is `(order_id, line_number)`. Dates use the retailer’s business calendar. All prices are USD in this example.

| Field | Type | Rule |
| --- | --- | --- |
| `order_id` | String | Required synthetic order identifier |
| `line_number` | Integer | Required, positive, and unique within the order |
| `booked_on` | Date | Required booking date |
| `product_id` | String | Required synthetic product identifier |
| `customer_version_id` | Integer | Required, identifies the single customer version effective at booking |
| `segment` | String | Required segment from that customer version |
| `quantity` | Integer | Required and positive |
| `unit_price_usd` | Decimal (12,2) | Required and nonnegative |
| `booked_value_usd` | Decimal, scale 2 | Quantity multiplied by unit price, excluding taxes and adjustments |

**Publication criteria** · The candidate must pass key, validity-window, history-match, and source reconciliation checks. The proposed freshness target is a validated publication by 08:00 UTC daily. A missed target triggers a freshness notice with the timestamp of the last successful publication.

**Change policy** · Review any change with consumers. A change to grain, metric definition, column meaning, or required fields needs a new major version and an agreed migration period. An additive optional field can ship in a minor version after compatibility review. A source correction that changes an already-published result follows the restatement decision in Decision 001.
