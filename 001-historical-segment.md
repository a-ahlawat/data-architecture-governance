# ADR 001 · Historical customer segment

**Status** · Accepted for this case study

**Context** · Finance needs booked value grouped by the segment effective on the booking date. A customer’s current segment can differ from their historical segment. Segment is supplied as a daily business effective date.

**Decision** · Maintain Type 2 history with start-inclusive, end-exclusive windows. Resolve the dimension at booking time using the business date. Validate that each booked line has exactly one match.

**Options considered**

| Option | Trade-off |
| --- | --- |
| Join to current customer segment | Simple to maintain, but a routine segment change would move older bookings between groups. |
| Persist segment directly on the booked line | Stable for a booking-time capture policy, but retroactive source corrections would require rewriting affected lines. |
| Resolve against effective-dated history | Supports historical analysis and controlled restatements. It requires trustworthy date windows and an explicit policy for corrections. |

**Consequences** · The validated snapshot preserves the segment values used for that publication. A historical correction triggers validation of affected periods and an owner-approved restatement with a new publication ID. Capture source batch IDs alongside publication IDs so a consumer can identify which version produced a result.

**Revisit when** · The source can supply reliable intraday changes, or audit users need to query what was known on any prior date independently of published snapshots. Evaluate timestamp grain or bitemporal history at that point.
