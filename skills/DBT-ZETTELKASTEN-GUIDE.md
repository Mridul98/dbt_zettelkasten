# CLI Zettelkasten for Data Engineering & dbt

Welcome to your Second Brain, optimized for Data Engineering and mapping out dbt repositories. This system blends the atomic, linked nature of a Zettelkasten with a hierarchical directory structure to ensure your notes mirror your data architecture.

## 1. Core Principles

### File Naming (The UID System)
Every file must start with a timestamp **Unique Identifier (UID)**.
* **Format:** `YYYYMMDDHHMM-[prefix]_descriptive_title_[suffix].md`
* **Conventions:**
  * Staging: `stg_` prefix (e.g., `202310270915-stg_stripe_payments.md`)
  * Intermediate: `int_` prefix (e.g., `202310271030-int_sales_pivoted.md`)
  * Reporting: `_report` suffix (e.g., `202310271400-monthly_revenue_report.md`)
* **Why:** UIDs prevent naming collisions. Strict prefixes/suffixes allow you to instantly identify the model's layer and enable lightning-fast global searches using wildcards (e.g., `*_report.md`).

### Markdown Linking (The DAG)
Links between notes represent your dbt DAG (Directed Acyclic Graph).
* **Format:** Standard Markdown `[Visible Text](./relative/path/to/file.md)`
* **Example:** `Depends on: [stg_stripe_payments](../staging/202310270915-stg-stripe-payments.md)`
* **Why:** Standard links work everywhere (GitHub, VSCode, Neovim, standard markdown renderers) and force you to be conscious of the physical directory hierarchy.

---

## 2. The Hybrid Directory Structure

Unlike traditional flat Zettelkastens, we leverage directories to mirror data layers. This provides "free tagging" and scoped searches.

```text
zettelkasten_demo/
├── 01-fleeting/             # Model-centric scratchpad. Strictly for observations, questions, or anomalies regarding data logic and business rules within the dbt models (NO generic software engineering tasks).
├── 02-knowledge/            # Permanent notes distilling the core domain architecture and business logic of the specific data models (e.g., 'The Unified Sales Funnel Architecture').
├── 03-repos/                # dbt repository documentations.
│   └── [repo-name]/         
│       ├── 00-MOC.md        # Map of Content (Index for the repo).
│       ├── staging/         # 1:1 mapping of source data (prefix: stg_).
│       ├── intermediate/    # Complex transformation logic (prefix: int_).
│       ├── reporting/       # Final business-facing models (suffix: _report).
│       ├── macros/          # Reusable Jinja snippets.
│       └── metrics/         # Core business definitions.
└── templates/               # Reusable markdown layouts.
```

---

## 3. Adapting Notes for Data Engineering

Do not just copy and paste SQL. Use your Zettelkasten to explain the **Why** and **How**. Focus on complex `intermediate` and `reporting` models; avoid documenting trivial 1:1 staging models unless they contain tricky logic.

### A. Documenting Grain & Purpose
The most critical part of a data model is its grain. Every model note must define its row-level identity.
* *Example:* "One row per `user_id` per `month`."

### B. Capturing Column Lineage & Logic
Never write a column description without explaining *how* it is calculated and *where* it came from. Use standard markdown links to point directly to the upstream model.

*Format:*
* **`[column_name]`**: [Business description]
  * **Logic:** [The exact SQL or explanation, e.g., `COUNT(DISTINCT deal_id)`]
  * **Origin:** [[Link to upstream model]] -> `[upstream_column_name]`

---

## 4. Lifecycle Management (Preventing Drift)

A static Second Brain for a dynamic dbt codebase will fail. You must treat notes like code.

* **Last Verified Commit:** Always include a metadata tag at the top of your models: `Last Verified Commit: <hash>`. This tells you exactly how old the documentation is.
* **The Tombstone Pattern:** When a model is deleted in dbt, **do not delete the note.** 
  1. Rename the file: `202310270915-DEPRECATED-int-legacy.md`.
  2. Fill out the "Deprecation Notes" section explaining why it was removed and what replaced it.

---

## 5. CLI Query Engine Mastery

Because we use standard text and folders, your terminal is the ultimate search engine. Use `ripgrep` (`rg`) or `grep` for lightning-fast lookups.

### 🔍 Search by Model Dependencies (1st-Degree Lineage)
*Scenario: What models rely on this staging model?*
1. Get the UID of the staging model (e.g., `202310270915`).
2. Search the repo for that UID:
   ```bash
   rg "202310270915" 03-repos/sales-analytics/
   ```

### 🔍 Scoped Searches via Directories
*Scenario: What is the final business logic for the `amount` column?*
Instead of searching everywhere, scope your search strictly to the `reporting/` directory to filter out intermediate noise.
```bash
rg "amount" 03-repos/sales-analytics/reporting/
```

### 🔍 Find by Naming Convention (Prefix/Suffix)
*Scenario: List every reporting model across all repositories.*
```bash
rg -g "*_report.md" --files 03-repos/
```
*Scenario: Find where a staging model is first used in intermediate logic.*
```bash
rg "stg_stripe_payments" 03-repos/sales-analytics/intermediate/
```

### 🔍 Trace Column Origin
*Scenario: What upstream file provides the data for `deals_in_stage`?*
```bash
rg -A 2 "\*\*\`deals_in_stage\`\*\*" 03-repos/
```
*(This command finds the column name and prints the next 2 lines, instantly revealing its Logic and Origin).*

### 🔍 Find all "User" grained models
```bash
rg -A 1 "\*\*Grain:\*\*" 03-repos/ | rg -i "user"
```
*(Finds the Grain header and searches the subsequent line for 'user')*

---

Your Second Brain is now ready. Start by copying `templates/template-dbt-model.md` into one of the `03-repos` subdirectories and trace your first dbt model!

---

## 6. Mandatory Triad Documentation
When documenting a dbt repository, you must create a triad of notes for **every** model:
*   **Repository Note (`03-repos/`)**: The standard lineage and column definition.
*   **Fleeting Note (`01-fleeting/`)**: Immediate observations, anomalies, or technical debt spotted in the SQL. Use `template-fleeting.md`.
*   **Knowledge Note (`02-knowledge/`)**: The extracted, abstracted business logic or architectural pattern behind the model. Use `template-knowledge.md`.
