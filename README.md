# Azure-End-to-end-Data-Engineering-Project
A project to familarise myself with the Azure data engineering ecosystem

## 1. Project overview

- **Goal**: Build a scalable pipeline to ingest and transform  data.
- **Scenario**: Where the data comes from, who would use the outputs.
- **Architecture style**: Medallion (Bronze → Silver → Gold) / Delta Lake / batch + near real-time, etc.

## 2. Architecture

- **High-level flow**:
  1. Ingest data from `<source>` into Azure Data Lake Gen2 (Bronze).
  2. Transform with Databricks (Bronze → Silver → Gold).
  3. Expose curated tables to `<Synapse / SQL DB / Power BI>` for analytics.
- Include an image:
  - `docs/architecture.png` (add later and link as: `![Architecture](docs/architecture.png)`)

## 3. Tech stack

- Azure Data Factory (or Synapse Pipelines) – orchestration
- Azure Data Lake Storage Gen2 – data lake
- Azure Databricks + Spark – data transformations
- Azure SQL / Synapse / Power BI – serving layer
- Delta Lake / Parquet – storage format
- GitHub – version control and documentation
  
## 4. Issues I solved (debugging highlights)

### Missing columns in Databricks schema (Product_Name, Year)
- Problem: Raw CSV had 12 columns but the SQL table / ADF schema only had 10, so Databricks was missing `Product_Name` and `Year`.
- Fix: Updated the SQL table schema, re-imported schema in ADF, reran the copy, and verified the new columns in the lake/Databricks.

**What I learned**
- Schema mismatches can happen across source files, SQL staging tables, ADF datasets, and lake outputs.
- Fixing the schema upstream is better than patching it only in notebooks.

### Incremental load and watermark
- Problem: Watermark table failed to debug; incremental copy behaved like a full load and the stored procedure failed due to parameter configuration.
- Fix: Aligned Lookup outputs (`firstRow` vs `value[0]`), corrected the `UpdateWatermarkTable` parameter mapping, and validated that row counts and `water_table.last_load` update as expected.

**What I learned**
- Incremental pipelines depend heavily on control tables and exact activity outputs.
- Small configuration differences in ADF can change pipeline behaviour significantly.
