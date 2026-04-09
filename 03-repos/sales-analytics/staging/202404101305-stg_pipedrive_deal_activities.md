# stg_pipedrive_deal_activities
**UID:** 202404101305
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_activity_id`.
* **Purpose:** Fact table for specific activities (e.g., calls, emails) tied to deals.

## 2. Column Lineage & Logic
* **`deal_activity_id`**: Primary key for the activity instance.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activities_snapshot` -> `activity_id`
* **`deal_activity_type`**: The system type identifier.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activities_snapshot` -> `type`
* **`deal_activity_assigned_to_user`**: ID of the user executing the activity.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activities_snapshot` -> `assigned_to_user`
* **`deal_id`**: Associated deal.
  * **Logic:** Passthrough.
  * **Origin:** `pipedrive_deal_activities_snapshot` -> `deal_id`
* **`is_deal_activity_done`**: Boolean if the activity was completed.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activities_snapshot` -> `done`
* **`deal_activity_due_to`**: Deadline / Timestamp of the activity.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_activities_snapshot` -> `due_to`
* **`dbt_staging_last_updated_at`**: Metadata timestamp of extraction.
  * **Logic:** `NOW()`
  * **Origin:** System timestamp

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Reads from `pipedrive_deal_activities_snapshot`.
    * Filters out invalidated records (`dbt_valid_to IS NULL`).

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * `pipedrive_deal_activities_snapshot`