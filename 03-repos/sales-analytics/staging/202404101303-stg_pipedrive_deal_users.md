# stg_pipedrive_deal_users
**UID:** 202404101303
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_user_id`.
* **Purpose:** Dimension table containing information about Pipedrive CRM users (sales reps).

## 2. Column Lineage & Logic
* **`deal_user_id`**: Primary key for the user.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_users_snapshot` -> `id`
* **`deal_user_name`**: Name of the user.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_users_snapshot` -> `name`
* **`deal_user_email`**: Email of the user.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_users_snapshot` -> `email`
* **`deal_user_last_modified`**: Timestamp of last update.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_users_snapshot` -> `modified`

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Reads from `pipedrive_deal_users_snapshot`.
    * Materialized as `incremental` with `merge` strategy on `deal_user_id`.
    * Filters out invalidated records (`dbt_valid_to IS NULL`).

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * `pipedrive_deal_users_snapshot`