# Data Lake Example: Blend Workday Data with External Datasets through Snowflake

## Scenario

This use case scenario illustrates how to build insights in Snowflake by blending live Workday data with other data sources.

> **Note:** This is a hypothetical scenario on building data assets by blending Workday and non-Workday data sources. This task highlights the usage of Views, Dynamic, and Temporary tables to stage Live Workday data to build insights. Once the insights are generated, the stage tables are flushed to ensure any sensitive data that you may have used to generate insights is not persisted in Snowflake! 

Imagine you're a people analyst trying to answer one question: "Who in our organization is ready to work on our most critical initiatives?" You have Workday — full of employee data — but skills are scattered across self-entered profiles, job titles, and role descriptions. 

We have created a sample script that an analyst can develop to answer this question. The script takes the relevant workday data sources, scores it for reliability, maps it against what each initiative actually needs, and hands creates a ranked, ready-to-load talent list. No manual spreadsheets. No guessing. No moving data. All data is accessed using Views and Temporary tables and only the final results are persisted.

<img width="451" height="368" alt="image" src="https://github.com/user-attachments/assets/4dba7fca-3c1f-4392-82cd-f33d0a1c11a1" />

## Steps

1. Navigate to Script “Use\_Case\_Implementation\_Skills” in Legacy Notebooks  
     
   Navigation Bar \> Projects \> Legacy Notebooks \> “Use\_Case\_Implementation\_Skills”

<img width="677" height="615" alt="image" src="https://github.com/user-attachments/assets/61dd7bd6-4cea-4b34-9df0-1a2c2b6dd1b1" />


2. Select the **USE_CASE_IMPLEMENTATION_SKILLS** file. This action opens the SQLs in a python notebook in a separate Window. Review & execute all the SQL’s in the notebook.
 
3. Click drop down next to Play button, select “**Run all**”.

<img width="1102" height="484" alt="image" src="https://github.com/user-attachments/assets/437ceed8-c2ba-48c4-965b-b73d11194caf" />


> **Note**: This script uses Views and Temporary tables to generate the final data-set that blends both Live zero-copy Workday Tables and non Workday data sources. Any sensitive fields used in generating the final data-sets are never materialized in Snowflake.

## Result

The script draws from three Snowflake data sources:

**Workday (zero-copy)** — WORKDAY\_ZERO\_COPY\_DB.WORKDAY\_CORE — the primary source. **Workers, job profiles, the skill catalog, supervisory org hierarchy, and external skill mappings** all live here. Zero-copy means the script reads directly from Workday's Iceberg tables in Snowflake without duplicating any data.

**Skill adjacency graph** — EXTERNAL\_DATASETS.SKILLS\_TAXONOMY.SKILL\_ADJACENCY — a pre-loaded Snowflake table that links each Workday skill to its nearest equivalents in the O\*NET and ESCO taxonomies, with a similarity score per edge. This is what allows the script to match a worker who has *Python* against an initiative that requires *data engineering* — close enough counts, with a confidence penalty.

**Initiative registry** — EXTERNAL\_DATASETS.SKILLS\_TAXONOMY.INITIATIVE\_REGISTRY — a pre-loaded Snowflake table listing each active strategic initiative, the skills it requires, and the minimum match score a candidate must clear to appear in the output.

Once the sources are in place, the script runs in four stages. 

1. **Staging** pulls raw Workday tables into clean views — the skill catalog, active workers, job profiles, and org hierarchy. 

2. **Skill profiling** scores every worker's skills by reliability: self-reported data scores higher, job-profile-inferred data gets a small discount. 

3. **Matching** runs each worker's skills against every active initiative two ways — exact name match, or a "close enough" match via the skill adjacency graph — and the best score per worker-skill pair survives deduplication. 

4. **Rollup** collapses everything to one row per worker per initiative, joins in org hierarchy for Workday's security model, stamps each candidate as READY\_NOW or READY\_WITH\_DEVELOPMENT, and writes the final table back into Snowflake for reporting — and optionally into Workday via Prism ingestion.

The final data-set is stored exclusively in Snowflake.

Zero-copy technology enables run-time access to your sensitive data, eliminating the need for materialization in Snowflake or the creation of ETL pipeline.

<img width="848" height="623" alt="image" src="https://github.com/user-attachments/assets/3d16e69c-dd9d-414c-9880-3cfd1f14b826" />
