# stg_pipedrive_deal_changes
**UID:** 202404101200
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_id` per `change_time`.
* **Purpose:** Turns a raw “one-change-per-row” EAV log into a readable deal timeline, filling down values so we know the state of the deal at any given timestamp.

## 2. Column Lineage & Logic
* **`deal_id`**: The unique identifier for the deal.
  * **Logic:** Passthrough.
  * **Origin:** `postgres_public.deal_changes` -> `deal_id`
* **`change_time`**: The exact timestamp when a field was changed.
  * **Logic:** Passthrough.
  * **Origin:** `postgres_public.deal_changes` -> `change_time`
* **`stage_id`**: The ID of the pipeline stage.
  * **Logic:** Extracted from raw log via macro, then filled down using `FIRST_VALUE() OVER (PARTITION BY deal_id, stage_id_change_count ORDER BY change_time)`.
  * **Origin:** `postgres_public.deal_changes` -> JSON extraction
* **`user_id`**: The ID of the owner/user assigned to the deal.
  * **Logic:** Extracted from raw log via macro, then filled down using `FIRST_VALUE() OVER (PARTITION BY deal_id, user_id_change_count ORDER BY change_time)`.
  * **Origin:** `postgres_public.deal_changes` -> JSON extraction

## 3. Transformation Evolution (Chronological)
1. **CTE `marked_deal_changes`:** Uses a macro `transform_deal_changes` to extract changed values (like `stage_id`, `user_id`) from the raw `deal_changes` source. Handles incremental logic.
2. **CTE `grouped_marked_deal_changes`:** 
    * Creates "change count" groups for each attribute (e.g., `stage_id_change_count`) using `SUM(CASE...) OVER (PARTITION BY deal_id ORDER BY change_time)`.
3. **Final Select:** 
    * Uses `FIRST_VALUE()` across the groups created above to "fill down" missing values for all core attributes.

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * `postgres_public.deal_changes` (Raw Source)
* **Used By (Downstream):** 
  * [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md)