# Control matrix

| Control | Accountable role | Trigger and action | Evidence |
| --- | --- | --- | --- |
| Order-line integrity | Order Operations Steward | Before promotion, check duplicate keys and invalid line numbers, prices, or quantities. Block on any failure. | Check ID, failure count, candidate ID |
| Customer history integrity | Customer Data Steward | Before promotion, check unique version IDs, valid dates, overlapping windows, and exactly one booking-time match per line. | Check ID, failure count, candidate ID |
| Financial reconciliation | Order Analytics Product Owner | Compare candidate and source line counts and booked USD totals before promotion. Review any difference. | Source batch IDs, candidate ID, counts and totals |
| Freshness | Platform Operator | At 08:00 UTC daily, check publication status. Notify consumers and the owner if the refresh is late. | Last successful publication ID, timestamp, incident ID |
| Access | Platform Access Owner | Grant reporting access through a consumer role scoped to serving objects. Review grants quarterly and after role changes. | Role grant export, approver, review date |
| Classification and minimization | Order Analytics Product Owner | Confirm public fixtures are synthetic. In a real deployment, classify linkable identifiers as restricted under the proposed policy and keep direct identifiers out of reporting. | Classification record and field-level review |
| Retention and recovery | Platform Operator | Keep validated snapshots for a proposed 30 days. Test restoration of a previous publication and record recovery evidence. | Retention job log, restored publication ID, drill result |

## Incident example

A customer history batch introduces overlapping dates. The candidate fails the window and match checks. The platform operator holds publication, reports the candidate ID and failing check counts, and alerts the Customer Data Steward. The steward corrects the effective dates. The operator builds a fresh candidate, reruns all checks, and records the promotion. If the refresh missed its target, consumers also receive the updated publication timestamp.
