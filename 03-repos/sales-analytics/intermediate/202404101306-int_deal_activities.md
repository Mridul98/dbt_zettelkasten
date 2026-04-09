# int_deal_activities
**UID:** 202404101306
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_activity_id` and `deal_id`.
* **Purpose:** Enriches deal activities with activity type metadata (like stage mapping and active status).

## 2. Column Lineage & Logic
* **`deal_activity_id`**: Primary key for the activity instance.
  * **Logic:** Passthrough.
  * **Origin:** [stg_pipedrive_deal_activities](../staging/202404101305-stg_pipedrive_deal_activities.md) -> `deal_activity_id`
* **`deal_activity_type_stage`**: The funnel stage mapped to this activity type.
  * **Logic:** Looked up via join.
  * **Origin:** [stg_pipedrive_deal_activity_types](../staging/202404101301-stg_pipedrive_deal_activity_types.md) -> `deal_activity_type_stage`
* **`is_deal_activity_done`**: Boolean if the activity was completed.
  * **Logic:** Passthrough.
  * **Origin:** [stg_pipedrive_deal_activities](../staging/202404101305-stg_pipedrive_deal_activities.md) -> `is_deal_activity_done`
* **`deal_activity_due_to`**: Deadline / Timestamp.
  * **Logic:** Passthrough.
  * **Origin:** [stg_pipedrive_deal_activities](../staging/202404101305-stg_pipedrive_deal_activities.md) -> `deal_activity_due_to`
* **`dbt_staging_last_updated_at`**: Metadata timestamp used for incremental logic.
  * **Logic:** Passthrough.
  * **Origin:** [stg_pipedrive_deal_activities](../staging/202404101305-stg_pipedrive_deal_activities.md) -> `dbt_staging_last_updated_at`

## 3. Transformation Evolution (Chronological)
1. **Incremental Config:** 
    * Uses a dynamic `run_query` in Jinja to grab the `max(dbt_staging_last_updated_at)` from the target table to handle incremental loads efficiently.
2. **CTE `deal_activities`:** 
    * Reads from [stg_pipedrive_deal_activities](../staging/202404101305-stg_pipedrive_deal_activities.md).
    * Joins with [stg_pipedrive_deal_activity_types](../staging/202404101301-stg_pipedrive_deal_activity_types.md) on `deal_activity_type`.
    * Filters incrementally based on the max timestamp.
3. **Final Select:** 
    * Simple projection of the joined columns.

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * [stg_pipedrive_deal_activities](../staging/202404101305-stg_pipedrive_deal_activities.md)
  * [stg_pipedrive_deal_activity_types](../staging/202404101301-stg_pipedrive_deal_activity_types.md)
* **Used By (Downstream):** 
  * [int_sales_funnel](../intermediate/202404101309-int_sales_funnel.md) (assumed/will verify)