# Funnel KPI Mapping Rules
**UID:** 202404101530
**Type:** Permanent / Domain Knowledge

## Concept
Because the funnel is a hybrid of stages and activities, the business requires a centralized mapping to translate disparate CRM actions into standardized KPI steps (e.g., Step 1, Step 2.1).

Currently, this business logic is hardcoded directly into the SQL of the staging layer. For example, the string 'Sales Call 1' is strictly evaluated to funnel step '2.1'. 

## Practical Implementation
* **Activity Mapping:** Hardcoded via `CASE WHEN` in [stg_pipedrive_deal_activity_types](../03-repos/sales-analytics/staging/202404101301-stg_pipedrive_deal_activity_types.md).
* **Unified Lookup:** Consolidated into a master mapping table in [int_funnel_kpi_mapping](../03-repos/sales-analytics/intermediate/202404101308-int_funnel_kpi_mapping.md).

## Related Notes
* [The Unified Sales Funnel Architecture](./202404101500-the-unified-sales-funnel-architecture.md)