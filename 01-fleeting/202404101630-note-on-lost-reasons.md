# Note on Lost Reasons
**UID:** 202404101630
**Type:** Fleeting / Scratchpad

## Context
Reviewing the lineage of [stg_pipedrive_deal_lost_reason](../03-repos/sales-analytics/staging/202404101304-stg_pipedrive_deal_lost_reason.md).

## Observation / Question
This staging model successfully isolates lost reasons from the generic `deal_fields` table. However, looking at the DAG, it is currently an orphan. It is never joined downstream into the final funnel models or `int_deal_stage_progression`. 

Are we planning a separate churn/loss analysis mart, or did we just forget to join this context into the main funnel?

## Next Steps
- [ ] Check Jira for upcoming tickets related to Churn/Loss analysis.