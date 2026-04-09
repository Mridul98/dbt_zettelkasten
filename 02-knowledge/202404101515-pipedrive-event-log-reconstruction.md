# Pipedrive Event Log Reconstruction
**UID:** 202404101515
**Type:** Permanent / Domain Knowledge

## Concept
Pipedrive's raw `deal_changes` table operates as an Entity-Attribute-Value (EAV) event log. It only records the *changed* field at a specific timestamp. 

To perform historical analysis (like tracking stage progression), we must reconstruct the full state of a deal at any given time. If a user changes the `user_id` on Tuesday, we still need to know what `stage_id` the deal was in on Tuesday, even though the `stage_id` wasn't explicitly logged in that event.

## Practical Implementation
The architecture solves this using a "fill-down" window function strategy.
1. It partitions by the deal.
2. It creates a cumulative count of changes for each specific attribute (creating groups).
3. It uses `FIRST_VALUE()` over those groups to propagate the last known value forward in time.

* **Implementation Model:** [stg_pipedrive_deal_changes](../03-repos/sales-analytics/staging/202404101200-stg_pipedrive_deal_changes.md)

## Related Notes
* N/A