# stg_pipedrive_deal_activity_types
**UID:** 202404101301
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_activity_type_id`.
* **Purpose:** Provides a dimension table of activity types, explicitly mapping specific sales calls to funnel stages.

## 2. Column Lineage & Logic
* **`deal_activity_type_id`**: Primary key for the activity type.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activity_types_snapshot` -> `id`
* **`deal_activity_type_name`**: Human-readable name of the activity.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activity_types_snapshot` -> `name`
* **`deal_activity_type_stage`**: Mapped stage number for the funnel (e.g., '2.1' or '3.1').
  * **Logic:** `CASE WHEN name = 'Sales Call 1' THEN '2.1' WHEN name = 'Sales Call 2' THEN '3.1' ELSE NULL END`
  * **Origin:** `pipedrive_deal_activity_types_snapshot` -> `name`
* **`is_deal_activity_type_active`**: Boolean indicating if the type is currently active.
  * **Logic:** `COALESCE(active = 'Yes', FALSE)`
  * **Origin:** `pipedrive_deal_activity_types_snapshot` -> `active`
* **`deal_activity_type`**: System identifier for the type.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activity_types_snapshot` -> `type`

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Reads from `pipedrive_deal_activity_types_snapshot`.
    * Hardcodes business rules for stage mapping and active boolean status.
    * Filters out invalidated records (`dbt_valid_to IS NULL`).

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * `pipedrive_deal_activity_types_snapshot`
* **Used By (Downstream):** 
  * [int_deal_activities](../intermediate/202404101306-int_deal_activities.md) (assumed/will update if needed)