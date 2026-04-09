# Observation: Null Stage Names
**UID:** 202404101600
**Type:** Fleeting / Scratchpad

## Context
Reviewing the filtering logic in [stg_pipedrive_deal_stages](../03-repos/sales-analytics/staging/202404101230-stg_pipedrive_deal_stages.md).

## Observation / Question
Noticed we are filtering out `stage_name IS NOT NULL`. Why would Pipedrive have a valid `stage_id` without a name? 
Need to check with the CRM admins if this represents deleted stages that are ghosted in the API, or if it's just bad data entry from a specific legacy period.

## Next Steps
- [ ] Check raw snapshot data to see how many rows this filter is actually dropping.
- [ ] Ping CRM team to verify business definition of a null stage name.