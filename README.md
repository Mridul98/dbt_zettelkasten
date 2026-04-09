# Zettelkasten Walkthrough: Documenting `dbt_enpal_assessment`

This walkthrough demonstrates how to convert raw dbt SQL into structured, linked Zettelkasten notes using our DE-optimized workflow. 

We documented the pipeline responsible for tracking deal stage progression from the `dbt_enpal_assessment` project.

## Documented Models
We have successfully extracted the knowledge from the following models and placed them in the appropriate directories:

1. **`03-repos/sales-analytics/staging/202404101200-stg_pipedrive_deal_changes.md`**
   * *What we did:* We documented how raw event logs use window functions to backfill missing state to build a complete timeline.
2. **`03-repos/sales-analytics/staging/202404101230-stg_pipedrive_deal_stages.md`**
   * *What we did:* A simple staging model to fetch active stage names.
3. **`03-repos/sales-analytics/intermediate/202404101215-int_deal_stage_progression.md`**
   * *What we did:* We documented how the timeline from `stg_pipedrive_deal_changes` is aggregated using `MIN()` to find the *first entry* into a stage, and how it links back to the staging models.

---

## Proving the System (CLI Searching)

Now that these notes are saved, open your terminal and try out these searches within the `zettelkasten_demo` folder:

### 1. Tracing Column Evolution
**Scenario:** A stakeholder asks, *"Where is `first_entry_date_for_stage` calculated, and what does it actually mean?"*
Run this command:
```bash
rg -C 2 "first_entry_date_for_stage"
```
*Result:* Your CLI will instantly point you to the `int_deal_stage_progression` note, showing you the exact CTE where `MIN(change_time)` is transformed into that column!

### 2. Finding Downstream Dependencies
**Scenario:** You want to modify the window functions in `stg_pipedrive_deal_changes`. What will break?
1. Find its UID: `202404101200`
2. Search for the UID globally across all repos:
```bash
rg "202404101200" 03-repos/
```
*Result:* The search will return the `int_deal_stage_progression.md` note, proving that it relies on this staging model.

### 3. Finding Models by Prefix
**Scenario:** Show me all intermediate models we have documented.
```bash
rg -g "*int_*.md" --files 03-repos/
```

---
You have successfully built a Second Brain that understands dbt Lineage, grain, and column-level evolution!# dbt_zettelkasten
