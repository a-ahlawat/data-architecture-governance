# ADR 002 · Governed publishing

**Status** · Accepted for this case study

**Context** · A reporting view over mutable inputs can change immediately when source history is corrected. A failed match can make booked value disappear from a report.

**Decision** · Stage candidate data under a publication ID using fixed input batches. Validate that candidate, then promote that same immutable snapshot behind the reporting interface. Serialize promotion for the product and record the active and previous publication IDs. Grant reporting consumers access through the serving interface.

| Option | Trade-off |
| --- | --- |
| Live view over mutable inputs | Minimal storage and fast visibility. Consumers can see data that has yet to pass checks. |
| Publish valid lines and quarantine exceptions | Availability improves, while totals become incomplete and consumers must interpret exceptions. |
| Promote the full validated snapshot | Keeps totals reconciled at publication. A rejected candidate leaves consumers on the previous refresh and requires freshness alerts. |

**Failure handling** · A check failure leaves the active publication unchanged. The platform operator routes the failed check IDs and counts to the owner, corrects inputs with the steward, and reruns validation under a new candidate ID. If an active publication is later withdrawn, the operator can restore the previous validated snapshot and record the rollback.

**Revisit when** · The proposed freshness target becomes tighter than the time needed to validate a complete snapshot. Assess partition-level promotion with partition and cross-partition reconciliation.
